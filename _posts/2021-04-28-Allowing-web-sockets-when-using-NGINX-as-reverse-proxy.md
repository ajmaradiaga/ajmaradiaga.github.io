---
layout: post
title: Allowing web socket connection when using NGINX as reverse proxy
---

*In this blog post I will cover what is required to allow web socket connections to your application when using NGINX as a reverse proxy.*

Lately I've been looking at having a single place to manage all of my development projects. I normally use more than one computer to code and managing versions, software packages, keys across multiple computers was getting too complex. Hence why I started looking at [Visual Studio code server](https://github.com/cdr/code-server) to host all my development projects. The VS Code server allows you to run VS Code on any machine anywhere and access it in the browser. This is sort-of like running your own [Github codespace](https://github.com/features/codespaces) locally.

As I self-host most of the applications that I use, it was a matter of finding a docker image for VS Code server. Fortunately, [linuxserver.io](https://linuxserver.io) has published a docker image for the VS code server - https://hub.docker.com/r/linuxserver/code-server/. This is what I'm using in my [Unraid](https://unraid.net/) server to run the VS code server instance.

Once I was running VS Code server, I was able to access it locally, via the local IP address, e.g. [http://192.168.90.100:8443/](http://192.168.90.100:8443/). I can now VPN to my local network and access the VS Code server. That being said, I want to access the service from anywhere without the need of VPNing to my local network, e.g. `https://my-vs-code-server.company.ltd`. 

```yaml
server {
        listen  443 ssl;
        server_name     my-vs-code-server.company.ltd;
        keepalive_timeout       70;

        location / {
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
        
                proxy_pass http://192.168.90.100:8443;
        }
}
```
<p align="center"><i>nginx.conf initial server configuration</i></p>

As mentioned in the introduction, I use NGINX as a reverse proxy to grant access to different services I self-host. NGINX receives the request from the outside world and routes the connection to the local service. Above, a sample of the configuration added to my `nginx.conf` to allow configuration to the local service. When attempting to access the VS code server via `https://my-vs-code-server.company.ltd`, the connection was reaching NGINX but it the web socket connection was failing. I was able to see the error using the Web Developer > Console tools in my web browser. Below a sample of the console log.

```
Firefox can't establish a connection to the server at wss://my-vs-code-server.company.ltd/?reconnectionToken=731b9120-9851-43ad-81c6-2ec1d20af9e2&reconnection=false&skipWebSocketFrames=false. browserSocketFactory.ts:162:30
  ERR [remote-connection][Management   ][731b9…][initial][code.maradia.ga:80] socketFactory.connect() failed or timed out. Error: log.ts:296:11
  ERR Error: WebSocket close with status code 1006
    M remoteAuthorityResolver.ts:70
    y browserSocketFactory.ts:129
log.ts:296:11
  ERR [remote-connection] An error occurred in the very first connect attempt, it will be treated as a permanent error! Error: log.ts:296:11
  ERR Error: WebSocket close with status code 1006
    M remoteAuthorityResolver.ts:70
    y browserSocketFactory.ts:129
log.ts:296:11
  ERR Error: WebSocket close with status code 1006
    M remoteAuthorityResolver.ts:70
    y browserSocketFactory.ts:129
log.ts:296:11
 WARN Ignoring the error while validating workspace folder vscode-remote://my-vs-code-server.company.ltd/config/workspace - WebSocket close with status code 1006 log.ts:290:11
```
<p align="center"><i>Console log - Web socket errors</i></p>

To allow web sockets via NGINX, we need to add a couple of proxy headers: `Upgrade` and `Connection`. Below a sample of the server configuration include the new proxy headers. Once the new `Upgrade` and `Connection` headers are added, NGINX will allow web socket connections.

```yaml
# Map
map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
}

server {
        listen  443 ssl;
        server_name     my-vs-code-server.company.ltd;
        keepalive_timeout       70;

        location / {
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;

                # New headers
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection $connection_upgrade;

                proxy_http_version 1.1;

                proxy_pass http://192.168.90.100:8443;
        }
}
```
<p align="center"><i>nginx.conf configuration required to allow web sockets</i></p>

I hope this blog post helps you setting up/allowing web socket connections through NGINX whenever you have a service that requires this type of access. 

### References:
- NGINX as a WebSocket Proxy: [https://www.nginx.com/blog/websocket-nginx/](https://www.nginx.com/blog/websocket-nginx/)