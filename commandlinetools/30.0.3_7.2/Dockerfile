# Android sdk tools
FROM openjdk:11-jdk-slim

# args default
ARG ANDROID_SDK_TOOLS="8092744"
ARG GRADLE_VERSION="7.2"
ARG ANDROID_BUILD_TOOLS="30.0.3"
ARG USE_NDK="yes"

ENV DEBCONF_NOWARNINGS yes
ENV REPO_OS_OVERRIDE "linux"

RUN apt-get update --quiet --yes \
    && apt-get autoremove --quiet --yes \
    && apt-get install --quiet --yes wget \
    && apt-get install --quiet --yes unzip \
    && rm -rf /var/lib/apt/lists/*

RUN wget --quiet --output-document=android-sdk.zip https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip
RUN unzip -q -d android-sdk-linux android-sdk.zip
RUN rm -rf android-sdk.zip

RUN wget --quiet --output-document=gradle-bin.zip https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip
RUN unzip -q -d gradle gradle-bin.zip
RUN rm -rf gradle-bin.zip

ENV ANDROID_HOME $PWD/android-sdk-linux
ENV GRADLE_HOME $PWD/gradle/gradle-${GRADLE_VERSION}
ENV PATH $PATH:$ANDROID_HOME/cmdline-tools/bin:$GRADLE_HOME/bin

# agree sdkmanager licenses
RUN mkdir ~/.android && \
    touch ~/.android/repositories.cfg
RUN yes | sdkmanager --sdk_root=${ANDROID_HOME} --licenses >/dev/null

RUN sdkmanager --sdk_root=${ANDROID_HOME} "build-tools;${ANDROID_BUILD_TOOLS}" >/dev/null
RUN ANDROID_COMPILE_SDK="${ANDROID_BUILD_TOOLS%%.*}"; sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-${ANDROID_COMPILE_SDK}" >/dev/null
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platform-tools" >/dev/null
RUN if [ "${USE_NDK}" = "yes" ]; then \
      sdkmanager --sdk_root=${ANDROID_HOME} "ndk-bundle" >/dev/null; \
    fi
