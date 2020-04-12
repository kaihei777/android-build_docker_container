# android-build_docker_container

This is Docker image for the Android building apps.

Debian-based and includes default command line tools of Android SDK toools as well as all additional components.

https://hub.docker.com/r/kaihei777/android_build_docker_container

## Image Variants

### `kaihei777/android_build_docker_container:sdk-tools`

#### Additional components.

- sdk-tools-linux:4333796
- gradle:5.6.4

----

### `kaihei777/android_build_docker_container:sdk-tools-<version>`

#### Additional components.

- sdk-tools-linux:4333796
- gradle:5.6.4
- build-tools:<version>
- platforms:<version>
- platform-tools:<version>

```app/build.gradle
android {
  buildToolsVersion "28.0.3" // version
}
```

----

### `kaihei777/android_build_docker_container:commandlinetools`

#### Additional components.

- commandlinetools-linux:6200805
- gradle:5.6.4

----

### `kaihei777/android_build_docker_container:commandlinetools-<version>`

#### Additional components.

- commandlinetools-linux:6200805
- gradle:5.6.4
- build-tools:<version>
- platforms:<version>
- platform-tools:<version>

```app/build.gradle
android {
  buildToolsVersion "28.0.3" // version
}
```

----

## Usage

To use this image, pull from [docker hub/kaihei777](https://hub.docker.com/repository/docker/kaihei777/android_build_docker_container) and then run the following command:

```
docker pull kaihei777/android_build_docker_container:sdk-tools
docker pull kaihei777/android_build_docker_container:sdk-tools-<version>
```

```
docker pull kaihei777/android_build_docker_container:commandlinetools
docker pull kaihei777/android_build_docker_container:commandlinetools-<version>
```

## Components default versions

### sdk-tools tag
- sdk-tools-linux:4333796
- gradle:5.6.4

### commandlinetools tag
- commandlinetools-linux:6200805
- gradle:5.6.4

## Example

### gitlab-ci

```.gitlab-ci.yml
image: kaihei777/android_build_docker_container:sdk-tools

variables:
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