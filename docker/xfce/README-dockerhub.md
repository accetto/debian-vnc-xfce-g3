# Headless Debian/Xfce container with VNC/noVNC

## accetto/debian-vnc-xfce-g3

[User Guide][this-user-guide] - [GitHub][this-github] - [Dockerfile][this-dockerfile] - [Readme][this-readme-full] - [Changelog][this-changelog]

![badge-docker-pulls][badge-docker-pulls]
![badge-docker-stars][badge-docker-stars]
![badge-github-release][badge-github-release]

***

This Docker Hub repository contains Docker images for headless working.

The images are based on [Debian 11][docker-debian] and include [Xfce][xfce] desktop, [TigerVNC][tigervnc] server and [noVNC][novnc] client.

There is also a similar sibling repository [accetto/ubuntu-vnc-xfce-g3][accetto-dockerhub-ubuntu-vnc-xfce-g3], containing images based on [Ubuntu 22.04 LTS and 20.04 LTS][docker-ubuntu].

This [User guide][this-user-guide] describes the images and how to use them.

The following image tags are regularly built and published on Docker Hub:

<!-- markdownlint-disable MD052 -->

- `latest` based on `Debian 11`

    ![badge_latest_created][badge_latest_created]
    [![badge_latest_version-sticker][badge_latest_version-sticker]][link_latest_version-sticker-verbose]

<!-- markdownlint-enable MD052 -->

**Hint:** Clicking the version sticker badge reveals more information about the particular build.

The main features and components of the images in the default configuration are:

- lightweight [Xfce][xfce] desktop environment (Debian distribution)
- [sudo][sudo] support
- current version of JSON processor [jq][jq]
- current version of high-performance [TigerVNC][tigervnc] server and client
- current version of [noVNC][novnc] HTML5 clients (full and lite) (TCP port **6901**)
- popular text editor [nano][nano] (Debian distribution)
- lite but advanced graphical editor [mousepad][mousepad] (Debian distribution)
- current version of [tini][tini] as the entry-point initial process (PID 1)
- support for overriding environment variables, VNC parameters, user and group (see [User guide][this-user-guide-using-containers])
- support of **version sticker** (see [User guide][this-user-guide-version-sticker])

The following **TCP** ports are exposed by default:

- **5901** for access over **VNC** (using VNC viewer)
- **6901** for access over [noVNC][novnc] (using web browser)

![container-screenshot][this-screenshot-container]

This is the **third generation** (G3) of my headless images.
The **second generation** (G2) contains the GitHub repository [accetto/xubuntu-vnc-novnc][accetto-github-xubuntu-vnc-novnc].
The **first generation** (G1) contains the GitHub repository [accetto/ubuntu-vnc-xfce][accetto-github-ubuntu-vnc-xfce].

If you've found a problem or you just have a question, please check the [User guide][this-user-guide], [Issues][this-issues] and [sibling Wiki][sibling-wiki] first.
Please do not overlook the closed issues.

If you do not find a solution, you can file a new issue.
The better you describe the problem, the bigger the chance it'll be solved soon.

If you have a question or an idea and you don't want to open an issue, you can also use the [sibling Discussions][sibling-discussions].

**Remark:** The [GitHub project][this-github] contains image generators that image users generally don’t need, unless they want to build the images themselves.

***

[this-user-guide]: https://accetto.github.io/user-guide-g3/

[this-user-guide-version-sticker]: https://accetto.github.io/user-guide-g3/version-sticker/

[this-user-guide-using-containers]: https://accetto.github.io/user-guide-g3/using-containers/

[this-changelog]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/CHANGELOG.md

[this-github]: https://github.com/accetto/debian-vnc-xfce-g3/

[this-readme-full]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/xfce/README.md

[this-issues]: https://github.com/accetto/debian-vnc-xfce-g3/issues

[this-dockerfile]: https://github.com/accetto/debian-vnc-xfce-g3/blob/master/docker/Dockerfile.xfce

[this-screenshot-container]: https://raw.githubusercontent.com/accetto/debian-vnc-xfce-g3/master/docker/doc/images/debian-vnc-xfce-g3-latest.png

[accetto-dockerhub-ubuntu-vnc-xfce-g3]: https://hub.docker.com/r/accetto/ubuntu-vnc-xfce-g3

[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki

[sibling-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions

[accetto-github-xubuntu-vnc-novnc]: https://github.com/accetto/xubuntu-vnc-novnc/

[accetto-github-ubuntu-vnc-xfce]: https://github.com/accetto/ubuntu-vnc-xfce

[docker-debian]: https://hub.docker.com/_/debian/
[docker-ubuntu]: https://hub.docker.com/_/ubuntu/

[jq]: https://stedolan.github.io/jq/
[mousepad]: https://github.com/codebrainz/mousepad
[nano]: https://www.nano-editor.org/
[novnc]: https://github.com/kanaka/noVNC
[sudo]: https://www.sudo.ws/
[tigervnc]: http://tigervnc.org
[tini]: https://github.com/krallin/tini
[xfce]: http://www.xfce.org

[badge-github-release]: https://badgen.net/github/release/accetto/debian-vnc-xfce-g3?icon=github&label=GitHub

[badge-docker-pulls]: https://badgen.net/docker/pulls/accetto/debian-vnc-xfce-g3?icon=docker&label=pulls

[badge-docker-stars]: https://badgen.net/docker/stars/accetto/debian-vnc-xfce-g3?icon=docker&label=stars

<!-- Appendix will be added by util-readme.sh -->
