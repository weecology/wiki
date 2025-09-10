---
title: "Containers"
summary: " "
---

Containers are a tool for running code in self-contained environments.

## Debugging r-devel using Docker

First [install docker](https://www.docker.com/get-started) for your operating system.

Then get an r-devel container:

```sh
sudo docker pull rocker/drd
```

The run docker interactively while mounting your working directory:

```sh
sudo docker run -v WORKING/DIRECTORY:/mnt/WORKDIR -it rocker/drd:latest
```

Replace `WORKING/DIRECTORY` with the path to the directory whose files you want to access and `WORKDIR` with whatever you want it to be named inside the `/mnt/` directory in the container.

This will open an interactive R console running r-devel.

## Using containers in VS Code

If you use VS Code as your IDE you can develop inside a container.
To setup this up follow [the official instructions](https://code.visualstudio.com/docs/devcontainers/containers), which can be summarized as:

1. Install docker for your OS with non-sudo access (on Linux add your user to the `docker` group and logout)
2. Install the Dev Containers extension
3. Create a `.devcontainers/devcontainer.json` file to your project directory indicating which container to use, e.g,.

```json
{
	"name": "r-devel",
	"image": "rocker/drd:latest"
}
```

This can be a little tricky to setup (don't be afraid to ask for help), but when you open the project in VS Code you'll be automatically working in the designated container.
