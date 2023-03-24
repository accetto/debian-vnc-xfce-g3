# CHANGELOG

## Project `accetto/debian-vnc-xfce-g3`

[Docker Hub][this-docker] - [Git Hub][this-github] - [sibling Wiki][sibling-wiki] - [sibling Discussions][sibling-discussions]

***

### Release 23.03.2

This release mitigates the problems with the edge use case, when users bind the whole `$HOME` directory to an external folder on the host computer.

Please note that I recommend to avoid doing that. If you really want to, then your best bet is using the Docker volumes. That is the only option I've found, which works across the environments. In the sibling discussion thread [#39](https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions/39) I've described the way, how to initialize a bound `$HOME` folder, if you really want to give it a try.

Main changes:

- file `.initial_sudo_password` has been moved from the `$HOME` to the `$STARTUPDIR` folder
- file `.initial_sudo_password` is not deleted, but cleared after the container user is created
- startup scripts have been adjusted and improved
- readme files have been updated

### Release 23.03.1

This is a maintenance release aiming to improve the scripts and documentation.

### Release 23.03

- updated with `TigerVNC 1.13.1` bugfix release
- also some updates in readme files

### Release 23.02

The initial version of the project has been derived from the sibling project [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3] (version G3v4, release 23.02.1).

***

[this-docker]: https://hub.docker.com/u/accetto/

[this-github]: https://github.com/accetto/debian-vnc-xfce-g3/

[accetto-github-ubuntu-vnc-xfce-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3

<!-- Sibling projects -->

[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki
[sibling-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions
