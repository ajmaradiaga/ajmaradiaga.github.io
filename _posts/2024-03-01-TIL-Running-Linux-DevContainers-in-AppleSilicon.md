---
layout: post
title: TIL - Running linux/amd64 Dev Containers on Apple Silicon
---

Today I learned how to force a [VSCode Dev Container](https://code.visualstudio.com/docs/devcontainers/create-dev-container) to run on `X86_64` architecture on my Mac. In essence, build a custom docker image by specifying a Dockerfile and in the FROM statement of the Dockerfile, specify the `--platform` flag. When you spin up the DevContainer it will automatically use the platform specified. If no platform is specified then the Dev Container will run on the platform of your OS, in my case `aarch64` and this was causing an issue when I was trying to run a Python program, as the program requires a [library](https://pypi.org/project/solace-pubsubplus/#files) that's only available for the `X86_64` architecture.

**Dockerfile**
```docker
FROM --platform=linux/amd64 python:3.12.2-slim-bullseye

CMD ["echo", "The python:3.12.2-slim-bullseye image is working!"]
```

**.devcontainer.json**
```jsonc
{
	"name": "Python 3 DockerHub",
	// Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
	// "image": "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye",

	"build":{
		"dockerfile": "Dockerfile"
	},

	// Features to add to the dev container. More info: https://containers.dev/features.
	// "features": {},

	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	// "forwardPorts": [],

	// Use 'postCreateCommand' to run commands after the container is created.
	"postCreateCommand": "pip3 install --user -r requirements.txt",

	// Configure tool-specific properties.
	// "customizations": {},

	// Uncomment to connect as root instead. More info: https://aka.ms/dev-containers-non-root.
	// "remoteUser": "root"
}
```
