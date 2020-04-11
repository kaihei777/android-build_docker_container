# Android sdk tools
FROM openjdk:8-jdk-slim

# args default
ARG ANDROID_COMPILE_SDK="28"
ARG ANDROID_BUILD_TOOLS="28.0.3"
ARG ANDROID_SDK_TOOLS="4333796"
ARG GRADLE_VERSION="5.6.4"
ARG USE_NDK="no"

ENV DEBCONF_NOWARNINGS yes
ENV REPO_OS_OVERRIDE "linux"

RUN apt-get update --quiet --yes \
    && apt-get autoremove --quiet --yes \
    && apt-get install --quiet --yes wget=1.20.1-1.1 \
    && apt-get install --quiet --yes unzip=6.0-23+deb10u1 \
    && rm -rf /var/lib/apt/lists/*

RUN wget --quiet --output-document=android-sdk.zip https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_SDK_TOOLS}.zip
RUN unzip -q -d android-sdk-linux android-sdk.zip
RUN rm -rf android-sdk.zip


RUN wget --quiet --output-document=gradle-bin.zip https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip
RUN unzip -q -d gradle gradle-bin.zip
RUN rm -rf gradle-bin.zip

ENV ANDROID_HOME $PWD/android-sdk-linux
ENV ANDROID_SDK_ROOT $ANDROID_HOME/tools
ENV GRADLE_HOME $PWD/gradle/gradle-${GRADLE_VERSION}
ENV PATH $PATH:$ANDROID_SDK_ROOT/bin:$GRADLE_HOME/bin

# agree sdkmanager licenses
RUN mkdir ~/.android && \
    touch ~/.android/repositories.cfg
RUN yes | sdkmanager --licenses >/dev/null


RUN sdkmanager "build-tools;${ANDROID_BUILD_TOOLS}" >/dev/null
RUN sdkmanager "platforms;android-${ANDROID_COMPILE_SDK}" >/dev/null
RUN sdkmanager "platform-tools" >/dev/null
RUN if [ "${USE_NDK}" = "yes" ]; then \
      RUN sdkmanager "ndk-bundle" >/dev/null; \
    fi
