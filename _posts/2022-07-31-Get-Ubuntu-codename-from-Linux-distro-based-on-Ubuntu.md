---
layout: post
title: TIL - How to get Ubuntu codename from Linux distro based on Ubuntu
---

I've been playing a bit with [Elementary OS](https://elementary.io/)... it is a nice, simple Linux distro based on Ubuntu. Which means that you you can follow the instructions for Ubuntu whenever you need to install any software in Elementary OS. That said, some installation instructions require specifying the Ubuntu codename, to be able to retrieve the right binary from internet, e.g. [Installing Docker Engine](https://docs.docker.com/engine/install/ubuntu/). A command commonly used to retrieve the codename of your distro is `lsb_release -cs`.

Unfortunately, when running this command in Elementary OS, it will get you the Elementary OS codename, in my case `jolnr`.  If I just run the command below, to setup the Docker repository, I will get an HTTP 404 error when running `apt update`. That codename doesn't exist in the list of [Docker Ubuntu repositories](https://download.docker.com/linux/ubuntu/dists/).

```bash
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable"
```

Which brings me to my question.... how do I get the Ubuntu codename, on which the Linux distro that I'm running, is based on?

```bash
$ cat /etc/os-release | grep UBUNTU
```

In Elementary OS, you can get the UBUNTU_CODENAME by getting the contents of /etc/os-release.

![os-release]({{ site.baseurl}}/images/os-release.png)

### References: 
- Install Docker Engine on Ubuntu [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)
- os-release - Operating system identification [https://www.linux.org/docs/man5/os-release.html](https://www.linux.org/docs/man5/os-release.html)
