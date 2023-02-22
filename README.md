# Headless Debian/Xfce containers with VNC/noVNC

## Project `accetto/debian-vnc-xfce-g3`

Version: G3v1

***

[Docker Hub][this-docker] - [Changelog][this-changelog] - [sibling Wiki][sibling-wiki] - [sibling Discussions][sibling-discussions]

![badge-github-release][badge-github-release]
![badge-github-release-date][badge-github-release-date]
![badge-github-stars][badge-github-stars]
![badge-github-forks][badge-github-forks]
![badge-github-open-issues][badge-github-open-issues]
![badge-github-closed-issues][badge-github-closed-issues]
![badge-github-releases][badge-github-releases]
![badge-github-commits][badge-github-commits]
![badge-github-last-commit][badge-github-last-commit]

<!-- ![badge-github-workflow-dockerhub-autobuild][badge-github-workflow-dockerhub-autobuild] -->
<!-- ![badge-github-workflow-dockerhub-post-push][badge-github-workflow-dockerhub-post-push] -->

***

- [Headless Debian/Xfce containers with VNC/noVNC](#headless-debianxfce-containers-with-vncnovnc)
  - [Project `accetto/debian-vnc-xfce-g3`](#project-accettodebian-vnc-xfce-g3)
    - [Introduction](#introduction)
    - [TL;DR](#tldr)
      - [Installing packages](#installing-packages)
      - [Shared memory size](#shared-memory-size)
      - [Extending images](#extending-images)
      - [Building images](#building-images)
      - [Sharing devices](#sharing-devices)
    - [Image generations](#image-generations)
    - [Project versions](#project-versions)
    - [Project goals](#project-goals)
    - [Project features](#project-features)
  - [Issues, Wiki and Discussions](#issues-wiki-and-discussions)
  - [Credits](#credits)

### Introduction

This repository contains resources for building Docker images based on [Debian 11][docker-debian] with [Xfce][xfce] desktop environment and [VNC][tigervnc]/[noVNC][novnc] servers for headless use.

The resources for the individual images and their variations (tags) are stored in the subfolders of the **master** branch. Each image has its own README file describing its features and usage.

The repository has been derived from the sibling project [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3] containing similar images based on [Ubuntu 22.04 LTS and 20.04 LTS][docker-ubuntu].

### TL;DR

There are currently resources for the following Docker images:

- [accetto/debian-vnc-xfce-g3][accetto-docker-debian-vnc-xfce-g3]
  - [full Readme][this-readme-image-base]
  - [Dockerfile][this-dockerfile] (common for all images)
  - [sibling Dockerfile stages diagram][sibling-diagram-dockerfile-stages] (common for all images)
- [accetto/debian-vnc-xfce-chromium-g3][accetto-docker-debian-vnc-xfce-chromium-g3]
  - [full Readme][this-readme-image-chromium]
- [accetto/debian-vnc-xfce-firefox-g3][accetto-docker-debian-vnc-xfce-firefox-g3]
  - [full Readme][this-readme-image-firefox]

#### Installing packages

I try to keep the images slim. Consequently you can sometimes encounter missing dependencies while adding more applications yourself. You can track the missing libraries on the [Debian Packages Search][debian-packages-search] page and install them subsequently.

You can also try to fix it by executing the following (the default `sudo` password is **headless**):

```shell
### apt cache needs to be updated only once
sudo apt-get update

sudo apt --fix-broken install
```

#### Shared memory size

Note that some applications require larger shared memory than the default 64MB. Using 256MB usually solves crashes or strange behavior.

You can check the current shared memory size by executing the following command inside the container:

```shell
df -h /dev/shm
```

The older sibling Wiki page [Firefox multi-process][that-wiki-firefox-multiprocess] describes several ways, how to increase the shared memory size.

#### Extending images

The provided example file `Dockerfile.extend` shows how to use the images as the base for your own images.

Your concrete `Dockerfile` may need more statements, but the concept should be clear.

The compose file `example.yml` shows how to switch to another non-root user and how to set the VNC password and resolution.

#### Building images

The fastest way to build the images:

```shell
### PWD = project root
### prepare and source the 'secrets.rc' file first (see 'example-secrets.rc')

### examples of building and publishing the individual images
./builder.sh latest all
./builder.sh latest-chromium all
./builder.sh latest-firefox all

### just building the images, skipping the publishing and the version sticker update
./builder.sh latest build
./builder.sh latest-chromium build
./builder.sh latest-firefox build

### examples of building and publishing the groups of images
./ci-builder.sh all group latest
./ci-builder.sh all group latest-chromium
./ci-builder.sh all group latest-firefox

### or all the images at once
./ci-builder.sh all group complete

### or skipping the publishing to the Docker Hub
./ci-builder.sh all-no-push group complete

### and so on
```

You can still execute the individual hook scripts as before (see the folder `/docker/hooks/`). However, the provided utilities `builder.sh` and `ci-builder.sh` are more convenient. Before pushing the images to the **Docker Hub** you have to prepare and source the file `secrets.rc` (see `example-secrets.rc`). The script `builder.sh` builds the individual images. The script `ci-builder.sh` can build various groups of images or all of them at once. Check the files `local-builder-readme.md`, `local-building-example.md` and the [sibling Wiki][sibling-wiki] for more information.

#### Sharing devices

Sharing the audio device for video with sound works only with `Chromium` and only on Linux:

```shell
docker run -it -P --rm \
  --device /dev/snd:/dev/snd:rw \
  --group-add audio \
accetto/debian-vnc-xfce-chromium-g3:latest
```

Sharing the display with the host works only on Linux:

```shell
xhost +local:$(whoami)

docker run -it -P --rm \
    -e DISPLAY=${DISPLAY} \
    --device /dev/dri/card0 \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
    accetto/debian-vnc-xfce-g3:latest --skip-vnc

xhost -local:$(whoami)
```

Sharing the X11 socket with the host works only on Linux:

```shell
xhost +local:$(whoami)

docker run -it -P --rm \
    --device /dev/dri/card0 \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
    accetto/debian-vnc-xfce-g3:latest

xhost -local:$(whoami)
```

### Image generations

This is the **third generation** (G3) of my headless images. The **second generation** (G2) contains the GitHub repository [accetto/xubuntu-vnc-novnc][accetto-github-xubuntu-vnc-novnc]. The **first generation** (G1) contains the GitHub repository [accetto/ubuntu-vnc-xfce][accetto-github-ubuntu-vnc-xfce].

### Project versions

This file describes the **first generation** (G3v1) of this project, which however corresponds to the **fourth version** (G3v4) of the **sibling project** [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3].

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-versions] to learn more about the older project versions.

### Project goals

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-goals] to learn more about the project goals.

### Project features

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-features] to learn more about the project features.

***

## Issues, Wiki and Discussions

If you have found a problem or you just have a question, please check the [Issues][this-issues] and the [sibling Wiki][sibling-wiki] first. Please do not overlook the closed issues.

If you do not find a solution, you can file a new issue. The better you describe the problem, the bigger the chance it'll be solved soon.

If you have a question or an idea and you don't want to open an issue, you can use the [sibling Discussions][sibling-discussions].

## Credits

Credit goes to all the countless people and companies, who contribute to open source community and make so many dreamy things real.

***

[this-docker]: https://hub.docker.com/u/accetto/

[this-changelog]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/CHANGELOG.md
<!-- [this-github]: https://github.com/accetto/debian-vnc-xfce-g3/ -->
[this-issues]: https://github.com/accetto/debian-vnc-xfce-g3/issues

[this-diagram-dockerfile-stages]: https://raw.githubusercontent.com/accetto/debian-vnc-xfce-g3/master/docker/doc/images/Dockerfile.xfce.png

[this-dockerfile]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/Dockerfile.xfce

[this-readme-image-base]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce/README.md
[this-readme-image-chromium]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce-chromium/README.md
[this-readme-image-firefox]: https://github.com/accetto/debian-vnc-xfce-g3/tree/master/docker/xfce-firefox

[accetto-docker-debian-vnc-xfce-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-g3
[accetto-docker-debian-vnc-xfce-chromium-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-chromium-g3
[accetto-docker-debian-vnc-xfce-firefox-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-firefox-g3

<!-- sibling projects -->

[accetto-github-ubuntu-vnc-xfce-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3
[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki
[sibling-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions

[accetto-github-ubuntu-vnc-xfce-g3_project-versions]: https://github.com/accetto/ubuntu-vnc-xfce-g3#project-versions
[accetto-github-ubuntu-vnc-xfce-g3_project-goals]: https://github.com/accetto/ubuntu-vnc-xfce-g3#project-goals
[accetto-github-ubuntu-vnc-xfce-g3_project-features]: https://github.com/accetto/ubuntu-vnc-xfce-g3#changes-and-new-features

[sibling-diagram-dockerfile-stages]: https://raw.githubusercontent.com/accetto/ubuntu-vnc-xfce-g3/master/docker/doc/images/Dockerfile.xfce.png

<!-- previous generations -->

[that-wiki-firefox-multiprocess]: https://github.com/accetto/xubuntu-vnc/wiki/Firefox-multiprocess
[accetto-github-xubuntu-vnc-novnc]: https://github.com/accetto/xubuntu-vnc-novnc/
[accetto-github-ubuntu-vnc-xfce]: https://github.com/accetto/ubuntu-vnc-xfce

<!-- external links -->

[docker-debian]: https://hub.docker.com/_/debian/
[docker-ubuntu]: https://hub.docker.com/_/ubuntu/

[debian-packages-search]: https://packages.debian.org/index

[novnc]: https://github.com/kanaka/noVNC
[tigervnc]: http://tigervnc.org
[xfce]: http://www.xfce.org

<!-- github badges -->

[badge-github-release]: https://badgen.net/github/release/accetto/debian-vnc-xfce-g3?icon=github&label=release

[badge-github-release-date]: https://img.shields.io/github/release-date/accetto/debian-vnc-xfce-g3?logo=github

[badge-github-stars]: https://badgen.net/github/stars/accetto/debian-vnc-xfce-g3?icon=github&label=stars

[badge-github-forks]: https://badgen.net/github/forks/accetto/debian-vnc-xfce-g3?icon=github&label=forks

[badge-github-releases]: https://badgen.net/github/releases/accetto/debian-vnc-xfce-g3?icon=github&label=releases

[badge-github-commits]: https://badgen.net/github/commits/accetto/debian-vnc-xfce-g3?icon=github&label=commits

[badge-github-last-commit]: https://badgen.net/github/last-commit/accetto/debian-vnc-xfce-g3?icon=github&label=last%20commit

[badge-github-closed-issues]: https://badgen.net/github/closed-issues/accetto/debian-vnc-xfce-g3?icon=github&label=closed%20issues

[badge-github-open-issues]: https://badgen.net/github/open-issues/accetto/debian-vnc-xfce-g3?icon=github&label=open%20issues
