FROM ubuntu:16.04
MAINTAINER Sam Davis <sam.davis@techngs.com>

# Install apt packages required for the container.
RUN apt-get update \
&& apt-get install -y \
git \
lib32stdc++6 \
lib32z1 \
npm \
nodejs \
nodejs-legacy \
s3cmd \
build-essential \
curl \
unzip \
openjdk-8-jdk-headless \
sendemail \
libio-socket-ssl-perl \
libnet-ssleay-perl \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

#Install Android SDK Tools
RUN cd /opt \
&& curl https://dl.google.com/android/repository/tools_r25.2.3-linux.zip -o android-sdk.zip \
&& mkdir android-sdk-linux \
&& unzip android-sdk.zip -d android-sdk-linux/ \
&& rm android-sdk.zip

#Set android home environment variable.
ENV ANDROID_HOME /opt/android-sdk-linux

#Install all the specific android sdk components that are actually needed
### For reference , this are listed by name in the Android-SDK-Manager that you get with
### Android Studio or with the stand-alone sdk tools.
RUN echo 'y' | /opt/android-sdk-linux/tools/android update sdk -u -a -t \
platform-tools,\
build-tools-25.0.2,\
android-25,\
extra-android-support,\
extra-google-m2repository,\
extra-android-m2repository

# Install npm packages required for Cordova and Ionic
RUN npm i -g \
cordova \
ionic \
gulp \
bower \
grunt \
phonegap \
&& npm cache clean

#Install Cordova Platforms (If we ever had a MAC docker container would be awesome to put it here)
RUN cordova platform add android
#Install cordova plugins
RUN cordova plugin add cordova-plugin-actionsheet --save \
&& cordova plugin add cordova-plugin-compat --save \
&& cordova plugin add cordova-plugin-geolocation --save \
&& cordova plugin add cordova-plugin-mauron85-background-geolocation --save \
&& cordova plugin add cordova-plugin-vibration --save \
&& cordova plugin add cordova-plugin-whitelist --save \
&& cordova plugin add cordova-plugin-x-toast --save \
&& cordova plugin add uk.co.workingedge.phonegap.plugin.launchnavigator --save \

# Install ionic plugins
RUN bower install ionic-calendar --allow-root --save \
&& bower install ionic-filter-bar --allow-root --save \
&& bower install ionic-numberpicker --allow-root --save

# Create dummy app to build and preload gradle and maven dependencies
RUN cd / \
&& echo 'n' | ionic start app \
&& cd /app \
&& ionic platform add android \
&& ionic build android \
&& rm -rf * .??* \
&& rm /root/.android/debug.keystore

#Normal Pre Build Steps
#bower install --allow-root
#ionic config build
#ionic state restore
#ionic resources
#ionic build android
#cordova build android

WORKDIR /app
