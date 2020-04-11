# android-build_docker_container

This is Docker image for the Android building apps.

https://hub.docker.com/r/kaihei777/android_build_docker_container

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

## Example

### gitlab-ci

```.gitlab-ci.yml
image: kaihei777/android_build_docker_container:sdk-tools

variables:
  ANDROID_COMPILE_SDK: "28"
  ANDROID_BUILD_TOOLS: "28.0.3"
  ANDROID_SDK_TOOLS:   "4333796"
  ARTIFACTS_NAME: "android_app_$CI_PIPELINE_ID"
  ARTIFACTS_DIR: "app/build/outputs"

stages:
  - build

apkCreate:
  stage: build
  script:
    - gradle --stacktrace --no-build-cache :app:assembledRelease
  artifacts:
    name: $ARTIFACTS_NAME
    paths:
      - $ARTIFACTS_DIR
  when: manual
```

### local build

project directory

```android_app_project directory
android_app_project
├── app
│   ├── app.iml
│   ├── build.gradle
│   ├── libs
│   │   └── jxl.jar
│   ├── proguard-rules.pro
│   └── src
│       ├── androidTest
│       ├── debug
│       ├── main
│       │   ├── AndroidManifest.xml
│       │   ├── java
│       │   └── res
│       ├── release
│       └── test
│           └── java
├── build.gradle
├── docker
│   └── sdk-tools
│       ├── Dockerfile
│       └── docker-compose.yml
├── gradle.properties
├── android_app_project.iml
├── local.properties
└── settings.gradle
```

docker-compose.yml

```docker/sdk-tools/docker-compose.yml
version: '3'
services:
  build:
    container_name: android_app_build_sdk-tools
    image: kaihei777/android_build_docker_container:sdk-tools
    build: .
    environment:
      ANDROID_COMPILE_SDK: "28"
      ANDROID_BUILD_TOOLS: "28.0.3"
      ANDROID_SDK_TOOLS: "4333796"
      GRADLE_VERSION: "5.6.4"
      USE_NDK: "no"
    volumes:
      - ../../:/var/build
    working_dir: /var/build
    command: gradle --no-build-cache --stacktrace :app:assembleRelease
```

Android building apps on docker

```
android_app_project$ cd docker/sdk-tools
sdk-tools$ docker-compose up
```