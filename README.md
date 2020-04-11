# android-build_docker_container

This is Docker image for the Android building apps.

## Supported tags

- kaihei777/android_build_docker_container:sdk-tools: this image is Debian-based and includes default command line tools of Android SDK toools (sdk-tools-linux) as well as all additional components(gradle, build-tools, platforms, platform-tools).

- kaihei777/android_build_docker_container:commandlinetools: this image is Debian-based and includes default command line tools of Android SDK toools (commandlinetools-linux) as well as all additional components(gradle, build-tools, platforms, platform-tools).

→ Check out Docker Hub for available tags.

## Usage

To use this image, pull from [docker hub/kaihei777](https://hub.docker.com/repository/docker/kaihei777/android_build_docker_container) and then run the following command:

```
docker pull kaihei777/android_build_docker_container:sdk-tools
```

```
docker pull kaihei777/android_build_docker_container:commandlinetools
```

## Components default versions

### sdk-tools tag
- sdk-tools-linux:4333796
- gradle:5.6.4
- build-tools:28.0.3
- platforms & platform-tools:android-28

### commandlinetools tag
- commandlinetools-linux:6200805
- gradle:5.6.4
- build-tools:28.0.3
- platforms & platform-tools:android-28

→ Check out Docker Hub for Dockerfile.
