# Headless Debian/Xfce container with VNC/noVNC and Chromium Browser

## accetto/debian-vnc-xfce-chromium-g3

[Docker Hub][this-docker] - [Git Hub][this-github] - [Dockerfile][this-dockerfile] - [Full Readme][this-readme-full] - [Changelog][this-changelog] - [Project Readme][this-readme-project]

![badge-docker-pulls][badge-docker-pulls]
![badge-docker-stars][badge-docker-stars]
![badge-github-release][badge-github-release]
![badge-github-release-date][badge-github-release-date]

***

- [Headless Debian/Xfce container with VNC/noVNC and Chromium Browser](#headless-debianxfce-container-with-vncnovnc-and-chromium-browser)
  - [accetto/debian-vnc-xfce-chromium-g3](#accettodebian-vnc-xfce-chromium-g3)
    - [Introduction](#introduction)
    - [TL;DR](#tldr)
      - [Installing packages](#installing-packages)
      - [Shared memory size](#shared-memory-size)
      - [Extending images](#extending-images)
      - [Building images](#building-images)
      - [Sharing devices](#sharing-devices)
    - [Description](#description)
    - [Image tags](#image-tags)
    - [More information](#more-information)

***

### Introduction

This repository contains resources for building Docker images based on [Debian 11][docker-debian] with [Xfce][xfce] desktop environment and [VNC][tigervnc]/[noVNC][novnc] servers for headless use and the current [Chromium][chromium] web browser.

There is also a similar sibling image [accetto/ubuntu-vnc-xfce-chromium-g3][accetto-dockerhub-ubuntu-vnc-xfce-chromium-g3] based on [Ubuntu 22.04 LTS and 20.04 LTS][docker-ubuntu].

This is the **short README** version for the **Docker Hub**. There is also the [full-length README][this-readme-full] on the **GitHub**.

### TL;DR

#### Installing packages

I try to keep the images slim. Consequently you can encounter missing dependencies while adding more applications yourself. You can track the missing libraries on the [Debian Packages Search][debian-packages-search] page and install them subsequently.

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
./builder.sh latest-chromium all

### just building an image, skipping the publishing and the version sticker update
./builder.sh latest-chromium build

### examples of building and publishing the images as a group
./ci-builder.sh all group latest-chromium

### or also
./ci-builder.sh all family latest-chromium
```

You can still execute the individual hook scripts as before (see the folder `/docker/hooks/`). However, the provided utilities `builder.sh` and `ci-builder.sh` are more convenient. Before pushing the images to the **Docker Hub** you have to prepare and source the file `secrets.rc` (see `example-secrets.rc`). The script `builder.sh` builds the individual images. The script `ci-builder.sh` can build various groups of images or all of them at once. Check the [builder-utility-readme][this-builder-readme], [local-building-example][this-readme-local-building-example] and [sibling Wiki][sibling-wiki] for more information.

Note that selected features that are enabled by default can be explicitly disabled via environment variables. This allows to build even smaller images by excluding, for example, `noVNC`. See the [local-building-example][this-readme-local-building-example] for more information.

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
    accetto/debian-vnc-xfce-chromium-g3:latest --skip-vnc

xhost -local:$(whoami)
```

Sharing the X11 socket with the host works only on Linux:

```shell
xhost +local:$(whoami)

docker run -it -P --rm \
    --device /dev/dri/card0 \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
    accetto/debian-vnc-xfce-chromium-g3:latest

xhost -local:$(whoami)
```

### Description

**Attention:** The [Chromium Browser][chromium] in these images runs in the `--no-sandbox` mode. You should be aware of the implications. The image is intended for testing and development.

This is the **third generation** (G3) of my headless images. The **second generation** (G2) contains the GitHub repository [accetto/xubuntu-vnc-novnc][accetto-github-xubuntu-vnc-novnc]. The **first generation** (G1) contains the GitHub repository [accetto/ubuntu-vnc-xfce][accetto-github-ubuntu-vnc-xfce].

The main features and components of the images in the default configuration are:

- utilities **ping**, **wget**, **sudo** (Debian distribution)
- current version of JSON processor [jq][jq]
- light-weight [Xfce][xfce] desktop environment (Debian distribution)
- current version of high-performance [TigerVNC][tigervnc] server and client
- current version of [noVNC][novnc] HTML5 clients (full and lite) (TCP port **6901**)
- popular text editor [nano][nano] (Debian distribution)
- lite but advanced graphical editor [mousepad][mousepad] (Debian distribution)
- current version of [tini][tini] as the entry-point initial process (PID 1)
- support for overriding both the container user and the group
- support of **version sticker** (see the [full-length README][this-readme-full] on the **GitHub**)
- current version of [Chromium Browser][chromium] open-source web browser (Debian distribution)

The history of notable changes is documented in the [CHANGELOG][this-changelog].

![container-screenshot][this-screenshot-container]

### Image tags

The following image tags are regularly built and published on the **Docker Hub**:

- `latest` based on `Debian 11`

    ![badge_latest_created][badge_latest_created]
    [![badge_latest_version-sticker][badge_latest_version-sticker]][link_latest_version-sticker-verbose]

Clicking on the version sticker badge reveals more information about the actual configuration of the image.

### More information

More information about these images can be found in the [full-length README][this-readme-full] file on the GitHub.

***

<!-- GitHub project common -->

[this-changelog]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/CHANGELOG.md
[this-github]: https://github.com/accetto/debian-vnc-xfce-g3/
<!-- [this-issues]: https://github.com/accetto/debian-vnc-xfce-g3/issues -->
[this-readme-full]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce-chromium/README.md
[this-readme-project]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/README.md

[this-builder-readme]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-builder.md
[this-readme-local-building-example]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-local-building-example.md

<!-- Docker image specific -->

[this-docker]: https://hub.docker.com/r/accetto/debian-vnc-xfce-chromium-g3/
[this-dockerfile]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/Dockerfile.xfce

[this-screenshot-container]: https://raw.githubusercontent.com/accetto/debian-vnc-xfce-g3/master/docker/doc/images/debian-vnc-xfce-chromium.jpg

<!-- Sibling projects -->

[accetto-dockerhub-ubuntu-vnc-xfce-chromium-g3]: https://hub.docker.com/r/accetto/ubuntu-vnc-xfce-chromium-g3

[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki

<!-- Previous generations -->

[accetto-github-xubuntu-vnc-novnc]: https://github.com/accetto/xubuntu-vnc-novnc/
[accetto-github-ubuntu-vnc-xfce]: https://github.com/accetto/ubuntu-vnc-xfce
[that-wiki-firefox-multiprocess]: https://github.com/accetto/xubuntu-vnc/wiki/Firefox-multiprocess

<!-- External links -->

[docker-debian]: https://hub.docker.com/_/debian/
[docker-ubuntu]: https://hub.docker.com/_/ubuntu/

<!-- [docker-doc]: https://docs.docker.com/ -->
<!-- [docker-doc-managing-data]: https://docs.docker.com/storage/ -->

[debian-packages-search]: https://packages.debian.org/index

[jq]: https://stedolan.github.io/jq/
[mousepad]: https://github.com/codebrainz/mousepad
[nano]: https://www.nano-editor.org/
[novnc]: https://github.com/kanaka/noVNC
[tigervnc]: http://tigervnc.org
[tini]: https://github.com/krallin/tini
[xfce]: http://www.xfce.org

[chromium]: https://www.chromium.org/Home

<!-- github badges common -->

[badge-github-release]: https://badgen.net/github/release/accetto/debian-vnc-xfce-g3?icon=github&label=release

[badge-github-release-date]: https://img.shields.io/github/release-date/accetto/debian-vnc-xfce-g3?logo=github

<!-- docker badges specific -->

[badge-docker-pulls]: https://badgen.net/docker/pulls/accetto/debian-vnc-xfce-chromium-g3?icon=docker&label=pulls

[badge-docker-stars]: https://badgen.net/docker/stars/accetto/debian-vnc-xfce-chromium-g3?icon=docker&label=stars

<!-- Appendix -->
