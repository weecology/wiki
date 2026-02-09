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

## Using containers on HiPerGator

While many developers are familiar with Docker, HiPerGator uses a slightly different container system called [apptainer](https://docs.rc.ufl.edu/software/apps/apptainer/). Apptainer can run Docker containers by first converting them to a Singularity image (.sif), which is similar to an ISO if you've ever burned a disk or created a bootable USB drive. There is some convenience here that the image of the VM is an obvious portable file on disk, while with Docker it's not always obvious where things are stored. Interacting with this image file is very similar to Docker. You can read more information about Apptainer [here](https://apptainer.org/docs/user/latest/quick_start.html#interacting-with-images).

## Example: Open Drone Map

In this example we'll use Open Drone Map ([ODM](https://github.com/OpenDroneMap/ODM)). ODM can be run via a docker container to avoid a fairly complex installation process.

First, we need to [pull an image](https://apptainer.org/docs/user/latest/docker_and_oci.html).  On the cluster, we need to load the `apptainer` module first. Then we have to `pull` the image. This may take a while, so use `srun` or `sbatch` with a job file. The first argument is the (optional) path to the created image, the second argument is the docker repo ID:

```
module load aptainer
srun -t 1:00:00 apptainer pull /path/to/odm.sif docker://opendronemap/odm
```

### Launchihng the container

To run a container, call `srun apptainer run` which should run the [entrypoint](https://docs.docker.com/reference/dockerfile/#entrypoint) in the container. Like Docker, you can also launch a shell or execute any program that's installed in the container (`docker exec` -> `apptainer exec`).

### Mounting local directories

The most common argument you'll want is `--bind` which is similar to `-v` in Docker and lets you mount a local filesystem in the container. In our example, ODM will look for input files in the `/project` folder. If we create folder called `data/working_dir`, we can then _bind_ this directory to `/project` like `--bind "data/working_dir:/project"` (`--bind "host_path:container_path"`)

```bash
#!/bin/bash

... # Other SBATCH options

#SBATCH --job-name=odm-node
#SBATCH --nodes=1
#SBATCH --partition=hpg-turin
#SBATCH --cpus-per-task=16
#SBATCH --mem=64GB
#SBATCH --time=12:00:00
#SBATCH --gpus=1


module load apptainer

srun apptainer run --bind "${PROJECT_FOLDER}:/project" odm.sif --project-path /project --max-concurrency 16 --fast-orthophoto
```

Don't forget to add any other `SBATCH` flags you need.

### NVIDIA inside containers

Use the `--nv` flag to [pass NVIDIA GPUs](https://apptainer.org/docs/user/latest/gpu.html) to the container. This requires CUDA to be installed on the host system (i.e. `module load cuda` first).
