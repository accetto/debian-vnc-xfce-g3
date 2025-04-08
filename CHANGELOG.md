# CHANGELOG

## Project `accetto/debian-vnc-xfce-g3`

[User Guide][this-user-guide] - [Docker Hub][this-docker] - [Git Hub][this-github] - [sibling Wiki][sibling-wiki] - [sibling Discussions][sibling-discussions]

***

### Release 25.04

Availability checking of the `wget` utility has been added.
The utility is used by the `cache` hook script for downloading of selected packages into the `g3-cache` folders.
It's generally not available on Windows environments by default.
You can install it or to build on an environment, where the utility is available (e.g. WSL or Linux).

The checking can be skipped by setting the environment variable `IGNORE_MISSING_WGET=1`.

The selected packages still will be downloaded into a temporary image layer, but not into the project's
`.g3-cache` folder nor the shared one, defined by the variable `SHARED_G3_CACHE_PATH`.

Other changes:

- The `ci-builder.sh` script's command `log get errors` now lists building errors and also warnings.

Updated components:

- `noVNC` to version **1.6.0**
- `websockify` to version **0.13.0**

### Release 25.03 (G3v7)

This is the first `G3v7` release, bringing an improved building pipeline.

The helper script `ci-builder.sh` can build final images significantly faster, because the temporary helper images are used as external caches.

Internally, the helper image is built by the `pre_build` hook script and then used by the `build` hook script.

The helper image is now deleted by the `build` hook script and not the `pre_build` hook script as before.

The `Dockerfiles` got a new metadata label `any.accetto.built-by="docker"`.

#### Remarks

If you would build a final image without building also the helper image (e.g. by executing `builder.sh latest build`), then there could be an error message about trying to remove the non-existing helper image.
You can safely ignore the message.

For example:

```shell
### The next line would build the helper image, but it was not executed.
#./build.sh latest pre_build

./build.sh latest build

### then somewhere near the end of the log
Removing helper image
Error response from daemon: No such image: accetto/headless-debian-g3_latest-helper:latest
```

### Release 25.01

This is a maintenance release.

Updated components:

- `noVNC` to version **1.5.0**
- `websockify` to version **0.12.0**

#### Remarks about `xfce4-about`

The version of the currently running `Xfce4` can be checked by the utility `xfce4-about`.
It can be executed from the terminal window or by the start menu item `Applications/About Xfce`.

However, the utility requires the module `libGL.so.1`, which is excluded from some images, to keep them smaller.

The module can be added in running containers as follows:

```shell
sudo apt-get update
sudo install libgl1
```

### Release 24.09.1

This is a fix release, finishing the changes announced in the previous release.

Changes:

- Default user `headless:headless (1000:1000)` has been changed to `headless:headless (1001:1001)`.
  - This change has been only done to keep the containers uniform with the ones from the sibling `Ubuntu` projects.

### Release 24.09 (G3v6)

This is the first `G3v6` release.
However, it's a maintenance release and the version number has been increased just to keep it synchronized with the **sibling project** [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3].
The previous version `G3v5` will still be available in this repository as the branch `archived-generation-g3v5`.

Changes:

- **[Sorry, this one change actually comes in the next release.]** - Default user `headless:headless (1000:1000)` has been changed to `headless:headless (1001:1001)`.
  - This change has been only done to keep the containers uniform with the ones from the sibling `Ubuntu` projects.
- The directive `syntax=docker/dockerfile:experimental` has been removed from all Dockerfiles.
- The `noVNC` starting page has been updated in all images.
  - If no `noVNC Client` is selected, then the `Full Client` will start automatically in 10 seconds.
- The hook script `release_of` has been updated with the intention to report more helpful building errors.

### Release 24.03 (G3v5)

This is the first `G3v5` release.

*Remark*: The version numbers `G3v2`, `G3v3` and `G3v4` have been skipped, to align the numbering with the **sibling project** [accetto/ubuntu-vnc-xfce-g3][accetto-github-ubuntu-vnc-xfce-g3].

The updated script `set_user_permissions.sh`, which is part of Dockerfiles, skips the hidden files and directories now.
It generally should not have any unwanted side effects, but it may make a difference in some scenarios, hence the version increase.

### Release 23.12

This is a maintenance release.

- Updated Dockerfiles
  - file `.bashrc` is created earlier (stage `merge_stage_vnc`)
- Updated file `example-secrets.rc`
  - removed the initialization of the variables `FORCE_BUILDING` and `FORCE_PUBLISHING_BUILDER_REPO` (unset means `0`)
  - the variables are still used as before, but now they can be set individually for each building/publishing run

### Release 23.11

- Added file `$HOME/.bashrc` to all images.
It contains examples of custom aliases
  - `ll` - just `ls -l`
  - `cls` - clears the terminal window
  - `ps1` - sets the command prompt text

- Added more 'die-fast' error handling into the building and publishing scripts.
They exit immediately if the image building or pushing commands fail.

### Release 23.08.1 (Milestone)

This release brings new images based on the current **Debian 12**.
The images based on the previous **Debian 11** will still be published into the same repositories.

Other changes:

- hook scripts `env.rc`, `push` and `post_push` have been updated
- handling of multiple deployment tags per image has been improved and it covers also publishing into the builder repository now
  - also less image pollution by publishing
- file `readme-local-building-example.md` got a new section `Tips and examples`, containing
  - `How to deploy all images into one repository`

Main updated components:

- `Debian` to version **12.1**
- `Xfce` desktop to version **4.18**
- `Mousepad` to version **0.5.10**
- `nano` to version **7.2**
- `Python` to version **3.11.2**

### Release 23.08

This release brings updated and significantly shortened README files, because most of the content has been moved into the new [User guide][this-user-guide].

### Release 23.07.1

This release brings some enhancements in the Dockerfile and the script `user_generator.rc` with the aim to better support extending the images.

### Release 23.07

This release introduces a new feature `FEATURES_OVERRIDING_ENVV`, which controls the overriding or adding of environment variables at the container startup-time.
Meaning, after the container has already been created.

The feature is enabled by default.
It can be disabled by setting the variable `FEATURES_OVERRIDING_ENVV` to zero when the container is created or the image is built.
Be aware that any other value than zero, even if unset or empty, enables the feature.

If `FEATURES_OVERRIDING_ENVV=1`, then the container startup script will look for the file `$HOME/.override/.override_envv.rc` and source all the lines that begin with the string 'export ' at the first position and contain the '=' character.

The overriding file can be provided from outside the container using *bind mounts* or *volumes*.

The lines that have been actually sourced can be reported into the container's log if the startup parameter `--verbose` or `--debug` is provided.

This feature is an enhanced implementation of the previously available functionality known as **Overriding VNC/noVNC parameters at the container startup-time**.

Therefore this is a **breaking change** for the users that already use the VNC/noVNC overriding.
They need to move the content from the previous file `$HOME"/.vnc_override.rc` into the new file `$HOME/.override/.override_envv.rc`.

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

[this-user-guide]: https://accetto.github.io/user-guide-g3/

[this-docker]: https://hub.docker.com/u/accetto/

[this-github]: https://github.com/accetto/debian-vnc-xfce-g3/

[accetto-github-ubuntu-vnc-xfce-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3

[sibling-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki

[sibling-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions
