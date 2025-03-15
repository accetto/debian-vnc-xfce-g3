# Headless Debian/Xfce containers with VNC/noVNC

## Project `accetto/debian-vnc-xfce-g3`

Version: G3v7

***

[User Guide][this-user-guide] - [Docker Hub][this-docker] - [Changelog][this-changelog] - [sibling Wiki][sibling-wiki] - [sibling Discussions][sibling-discussions]

![badge-github-release][badge-github-release]
![badge-github-release-date][badge-github-release-date]
![badge-github-stars][badge-github-stars]
![badge-github-forks][badge-github-forks]
![badge-github-open-issues][badge-github-open-issues]
![badge-github-closed-issues][badge-github-closed-issues]
![badge-github-releases][badge-github-releases]
![badge-github-commits][badge-github-commits]
![badge-github-last-commit][badge-github-last-commit]

***

- [Headless Debian/Xfce containers with VNC/noVNC](#headless-debianxfce-containers-with-vncnovnc)
  - [Project `accetto/debian-vnc-xfce-g3`](#project-accettodebian-vnc-xfce-g3)
    - [Introduction](#introduction)
    - [Building images](#building-images)
    - [Image generations](#image-generations)
    - [Project versions](#project-versions)
      - [Previous versions](#previous-versions)
    - [Project goals](#project-goals)
    - [Project features](#project-features)
    - [How to fork](#how-to-fork)
    - [Getting help](#getting-help)
    - [Credits](#credits)

### Introduction

This GitHub repository contains resources and tools for building Docker images for headless working.

The images are based on the current [Debian 12][docker-debian] and the previous [Debian 11][docker-debian] and include [Xfce][xfce] desktop, [TigerVNC][tigervnc] server and [noVNC][novnc] client.
The popular web browsers [Chromium][chromium] and [Firefox][firefox] are also included.

This [User guide][this-user-guide] describes the images and how to use them.

The content of this GitHub project is intended for developers and image builders.

Ordinary users can simply use the images available in the following repositories on Docker Hub:

- [accetto/debian-vnc-xfce-g3][accetto-docker-debian-vnc-xfce-g3]
- [accetto/debian-vnc-xfce-chromium-g3][accetto-docker-debian-vnc-xfce-chromium-g3]
- [accetto/debian-vnc-xfce-firefox-g3][accetto-docker-debian-vnc-xfce-firefox-g3]

This project has been derived from the sibling project [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3] containing similar images based on [Ubuntu 22.04 LTS and 20.04 LTS][docker-ubuntu].

### Building images

You can execute the individual hook scripts in the folder [/docker/hooks/][this-folder-docker-hooks].
However, the provided utilities are more convenient.

The script [builder.sh][this-readme-builder] builds individual images.
The script [ci-builder.sh][this-readme-ci-builder] can build various groups of images or all of them at once.

Before building the images you have to prepare and source the file `secrets.rc` (see [example-secrets.rc][this-example-secrets-file]).

Features that are enabled by default can be explicitly disabled via environment variables.
This allows building even smaller images by excluding the individual features (e.g. noVNC).

The resources for building the individual images and their variations (tags) are in the subfolders of the [/docker/][this-folder-docker] folder.

The individual README files contain quick examples of building the images:

- [accetto/debian-vnc-xfce-g3][this-readme-debian-vnc-xfce-g3]
- [accetto/debian-vnc-xfce-chromium-g3][this-readme-debian-vnc-xfce-chromium-g3]
- [accetto/debian-vnc-xfce-firefox-g3][this-readme-debian-vnc-xfce-firefox-g3]

Each image also has a separate README file intended for Docker Hub.
The final files should be generated by the utility [util-readme.sh][this-readme-util-readme-examples] and then copied to Docker Hub manually.

The following resources describe the image building subject in details:

- [readme-local-building-example.md][this-readme-local-building-example]
- [readme-builder.md][this-readme-builder]
- [readme-ci-builder.md][this-readme-ci-builder]
- [readme-g3-cache.md][this-readme-g3-cache]
- [readme-util-readme-examples.md][this-readme-util-readme-examples]
- [sibling Wiki][sibling-wiki]

### Image generations

This is the **third generation** (G3) of my headless images.
The **second generation** (G2) contains the GitHub repository [accetto/xubuntu-vnc-novnc][accetto-github-xubuntu-vnc-novnc].
The **first generation** (G1) contains the GitHub repository [accetto/ubuntu-vnc-xfce][accetto-github-ubuntu-vnc-xfce].

### Project versions

This file describes the **seventh version** (G3v7) of the project.

This version brings an improved building pipeline.

The helper script `ci-builder.sh` can build final images significantly faster, because the temporary helper images are used as external caches.

Internally, the helper image is built by the `pre_build` hook script and then used by the `build` hook script.

The helper image is now removed by the `build` hook script and not the `pre_build` hook script.

However, also this version keeps evolving.
Please check the [CHANGELOG][this-changelog] for more information about the changes.

#### Previous versions

The previous versions are still available in this **GitHub** repository as the branches named as `archived-generation-g3v{d}`.

*Remark*: The version numbers `G3v2`, `G3v3` and `G3v4` have been skipped, to align the numbering with the **sibling project** [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3].

The main purpose of the version `G3v6` was to keep the project and the images uniform with the ones from the sibling `Ubuntu` projects.

The version `G3v5` has brought only one significant change comparing to the previous version `G3v1`.

- The updated script `set_user_permissions.sh`, which is part of Dockerfiles, skips the hidden files and directories now.
It generally should not have any unwanted side effects, but it may make a difference in some scenarios, hence the version increase.

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-versions] to learn more about the older project versions.

### Project goals

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-goals] to learn more about the project goals.

### Project features

Please refer to the [sibling project][accetto-github-ubuntu-vnc-xfce-g3_project-features] to learn more about the project features.

### How to fork

If you want to fork this project, then please check the page [How to fork this repository][sibling-wiki-how-to-fork] in the sibling [Wiki][sibling-wiki].

### Getting help

If you have found a problem or you just have a question, please check the [User guide][this-user-guide], [Issues][this-issues] and the [sibling Wiki][sibling-wiki] first.
Please do not overlook the closed issues.

If you do not find a solution, you can file a new issue.
The better you describe the problem, the bigger the chance it'll be solved soon.

If you have a question or an idea and you don't want to open an issue, you can use the [sibling Discussions][sibling-discussions].

### Credits

Credit goes to all the countless people and companies, who contribute to open source community and make so many dreamy things real.

***

[this-user-guide]: https://accetto.github.io/user-guide-g3/

[this-docker]: https://hub.docker.com/u/accetto/

[this-changelog]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/CHANGELOG.md

[this-issues]: https://github.com/accetto/debian-vnc-xfce-g3/issues

[this-folder-docker]: https://github.com/accetto/debian-vnc-xfce-g3/tree/master/docker

[this-folder-docker-hooks]: https://github.com/accetto/debian-vnc-xfce-g3/tree/master/docker/hooks

[this-example-secrets-file]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/examples/example-secrets.rc

[this-readme-debian-vnc-xfce-g3]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce/README.md

[this-readme-debian-vnc-xfce-chromium-g3]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce-chromium/README.md

[this-readme-debian-vnc-xfce-firefox-g3]: https://github.com/accetto/debian-vnc-xfce-g3/tree/master/docker/xfce-firefox

[this-readme-local-building-example]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-local-building-example.md

[this-readme-builder]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-builder.md

[this-readme-ci-builder]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-ci-builder.md

[this-readme-g3-cache]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/readme-g3-cache.md

[this-readme-util-readme-examples]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/utils/readme-util-readme-examples.md

[accetto-docker-debian-vnc-xfce-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-g3

[accetto-docker-debian-vnc-xfce-chromium-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-chromium-g3

[accetto-docker-debian-vnc-xfce-firefox-g3]: https://hub.docker.com/r/accetto/debian-vnc-xfce-firefox-g3

[accetto-github-ubuntu-vnc-xfce-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3

[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki

[sibling-wiki-how-to-fork]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki/How-to-fork

[sibling-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions

[accetto-github-ubuntu-vnc-xfce-g3_project-versions]: https://github.com/accetto/ubuntu-vnc-xfce-g3#project-versions

[accetto-github-ubuntu-vnc-xfce-g3_project-goals]: https://github.com/accetto/ubuntu-vnc-xfce-g3#project-goals

[accetto-github-ubuntu-vnc-xfce-g3_project-features]: https://github.com/accetto/ubuntu-vnc-xfce-g3#changes-and-new-features

[accetto-github-xubuntu-vnc-novnc]: https://github.com/accetto/xubuntu-vnc-novnc/

[accetto-github-ubuntu-vnc-xfce]: https://github.com/accetto/ubuntu-vnc-xfce

[docker-debian]: https://hub.docker.com/_/debian/
[docker-ubuntu]: https://hub.docker.com/_/ubuntu/

[chromium]: https://www.chromium.org/Home
[firefox]: https://www.mozilla.org
[novnc]: https://github.com/kanaka/noVNC
[tigervnc]: http://tigervnc.org
[xfce]: http://www.xfce.org

[badge-github-release]: https://badgen.net/github/release/accetto/debian-vnc-xfce-g3?icon=github&label=release

[badge-github-release-date]: https://img.shields.io/github/release-date/accetto/debian-vnc-xfce-g3?logo=github

[badge-github-stars]: https://badgen.net/github/stars/accetto/debian-vnc-xfce-g3?icon=github&label=stars

[badge-github-forks]: https://badgen.net/github/forks/accetto/debian-vnc-xfce-g3?icon=github&label=forks

[badge-github-releases]: https://badgen.net/github/releases/accetto/debian-vnc-xfce-g3?icon=github&label=releases

[badge-github-commits]: https://badgen.net/github/commits/accetto/debian-vnc-xfce-g3?icon=github&label=commits

[badge-github-last-commit]: https://badgen.net/github/last-commit/accetto/debian-vnc-xfce-g3?icon=github&label=last%20commit

[badge-github-closed-issues]: https://badgen.net/github/closed-issues/accetto/debian-vnc-xfce-g3?icon=github&label=closed%20issues

[badge-github-open-issues]: https://badgen.net/github/open-issues/accetto/debian-vnc-xfce-g3?icon=github&label=open%20issues
