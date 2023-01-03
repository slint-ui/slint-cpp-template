# Developing with Slint on Torizon

[Torizon](https://developer.toradex.com/torizon/) is a Linux-based software platform from Toradex that simplifies the process of developing and maintaining embedded software.

The files in this directory demonstrate how to use Torizon to develop and deploy a Slint application.

## Prerequisites

 - [Docker](https://www.docker.com) running on your development machine.
 - An embedded device that's running Toradex Torizon.
 - A docker hub account or a local docker registry accessible for pushing images to and to pull from to deploy to the device.

## Docker Setup

The provided docker file performs the following steps in a multi-stage build:

1. The Debian-based Torizon cross-compile environment is selected as base for the build.
2. Build tools such as cmake, ninja, rust, and cargo are installed.
3. Build dependencies for Slint are installed.
4. Slint is cloned from git, built, and installed. Slint is configured to use Wayland.
5. The application (parent directory of this file) is built as cmake application.
6. The Debian-based Torizon Wayland base image is selected as base for the deployment.
7. Run-time dependencies for Slint are installed.
8. The target build of Slint is copied from the build environment.
9. The application binary is copied from the build environment.

## Building

Set the following build arguments to configure the docker file to use the correct target architecture.

Torizon armhf Target Architecture:

 - TOOLCHAIN_ARCH=arm64
 - IMAGE_ARCH=linux/arm64
 - RUST_TOOLCHAIN_ARCH=aarch64-unknown-linux-gnu

Torizon arm64 Target Architecture:

 - TOOLCHAIN_ARCH=armhf
 - IMAGE_ARCH=linux/arm/v8
 - RUST_TOOLCHAIN_ARCH=armv7-unknown-linux-gnueabihf

If you're building with docker on the command line, the following example selects the arm64 architecture:

```sh
docker build -f torizon/Dockerfile --build-arg TOOLCHAIN_ARCH=arm64 --build-arg IMAGE_ARCH=linux/arm64 --build-arg RUST_TOOLCHAIN_ARCH=aarch64-unknown-linux-gnu -t <user-name-or-registry>/<my-container-name> .
```

## Deployment

In order to run the application, you need to pull the image to the device and run it.

If you've built the container on the command line, you first need to push it:

```sh
docker push <user-name-or-registry>/<my-container-name>
```

Before running the container, make sure that [weston is running in a container](https://developer.toradex.com/torizon/application-development/gui/qt-debian-container-for-torizon/#run-a-weston-container).

When running the container, make sure that to user the same user as with the weston container (`--user=torizon`) and forward the dev and tmp directories, for access to the GPU devices and the wayland compositor, for example:

```sh
docker run -d --user=torizon -v /dev:/dev -v /tmp:/tmp <user-name-or-registry>/<my-container-name>
```

## Customization

You may need to make changes to the docker file to customize how your application is run or to adapt it to different devices:

 - If you're using a device with a Vivate GPU, you may want to choose a different wayland base image for deployment. The default build argument for `BASE_IMAGE` is `wayland-base`, but `wayland-base-vivate` may be the correct choice then.
 - If you want to run your app in full-screen, pass `-e SLINT_FULLSCREEN=1` as additional argument when running the container.
