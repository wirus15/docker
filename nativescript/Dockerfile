FROM ubuntu:18.04
MAINTAINER Maciej Krawczyk <wirus15@gmail.com>

RUN useradd -ms /bin/bash nativescript

# Utilities
RUN apt-get update && \
    apt-get -y install apt-utils apt-transport-https unzip gnupg2 curl usbutils --no-install-recommends && \
    rm -r /var/lib/apt/lists/*

# Libs
RUN dpkg --add-architecture i386
RUN apt-get update && \
    apt-get -y install lib32z1 lib32ncurses5 libbz2-1.0:i386 libstdc++6:i386 g++

# NodeJS
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get update && \
    apt-get -y install nodejs npm build-essential && \
    rm -r /var/lib/apt/lists/*

# JAVA
RUN apt-get update && \
    apt-get -y install software-properties-common && \
    add-apt-repository ppa:webupd8team/java && \
    apt-get update && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get -y install oracle-java8-installer && \
    rm -r /var/lib/apt/lists/*
RUN export JAVA_HOME=$(update-alternatives --query javac | sed -n -e 's/Best: *\(.*\)\/bin\/javac/\1/p')

# NativeScript
RUN npm install -g --unsafe-perm nativescript@latest && \
    tns error-reporting disable

# Android SDK
ARG ANDROID_SDK_URL="https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip"
ADD $ANDROID_SDK_URL /tmp/android-sdk.zip
RUN mkdir /opt/android-sdk /app /dist && \
    chown nativescript:nativescript /tmp/android-sdk.zip /opt/android-sdk /app /dist
USER nativescript
ENV ANDROID_HOME /opt/android-sdk
ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
RUN unzip -q /tmp/android-sdk.zip -d /opt/android-sdk && \
    rm /tmp/android-sdk.zip
RUN yes | $ANDROID_HOME/tools/bin/sdkmanager "tools" "platform-tools" "platforms;android-25" "build-tools;27.0.3" "extras;android;m2repository" "extras;google;m2repository"

VOLUME ["/app","/dist"]

WORKDIR /app
