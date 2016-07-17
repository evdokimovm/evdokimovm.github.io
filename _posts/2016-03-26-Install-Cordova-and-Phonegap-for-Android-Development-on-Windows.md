---
layout: post
title: Install Cordova and Phonegap for Android Development on Windows
categories: [Cordova, Phonegap, Android]
tags: [Cordova, Phonegap, Android]
fullview: false
comments: true
description: In this post I will tell you how to install development environment to get started with Apache Cordova and Phonegap for Android Development on Windows. I'll tell you how to install Apache Cordova, Phonegap (and the difference between Cordova and Phonegap), Java SE Development Kit, Android SDK and Android SDK Tools with Android Studio and Apache Ant.
---

In this post I will tell you how to install development environment to get started with Apache Cordova and Phonegap for Android Development on Windows.

A development environment is a term that refers to having everything you need in order to develop, set up, and be ready to go in one place. We need the following things to get started:

1. Node.js and NPM
2. Java SE Development Kit
3. Android SDK and Android SDK Tools
4. Apache Ant
5. Git
6. Apache Cordova and Phonegap

#### Install Node.js and NPM

Cordova runs on the Node.js platform, which needs to be installed as the first step. For install Node.js, download installer from official web site [nodejs.org](https://nodejs.org/en/).
I use [Node.js 4.4.1 LTS x64](https://nodejs.org/dist/v4.4.1/node-v4.4.1-x64.msi).
After install Node.js is complete open Command Prompt and upgrade NPM (Node Package Manager) to latest version with command:

{% highlight text %}
npm install -g npm@3.8.3
{% endhighlight %}

> NPM is Node Package Manager for install Node.js packages from [npmjs.com](https://www.npmjs.com/). Including [Apache Cordova](https://www.npmjs.com/package/cordova), [Phonegap](https://www.npmjs.com/package/phonegap), [Ionic Framework](https://www.npmjs.com/package/ionic-framework) as well as plug-ins for Cordova and Phonegap.

#### Install Java SE Development Kit

This can be as simple as downloading, double-clicking on the downloaded file, and following the installation instructions. For install Java SE Development Kit download it from official web site. [Java SE Development Kit. Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

> If you use 64-bit Windows download JDK x64.

![JDK](https://i.imgur.com/7kgZqRN.png)

> During installation, just follow the instructions and do not change anything.

After JDK install is complete you need to add new **JAVA_HOME** system variable with path to your JDK.

Open **Control Panel > System and Security > System > Advanced System Settings**. Then in **System Properties** window select the **Advanced tab** and click the **Environment Variables** button. In the list **System Variables** click button **New...** then in **Variable name** field add **JAVA_HOME** and in **Variable value** field add path to your JDK. Click OK. It looks like this:

![JAVA_HOME](https://i.imgur.com/YQhVhO0.png)

Next In the list **User variables** select **PATH** and click the **Edit** button. At the end of the field **Variable value**, add a semicolon and path to the bin directory of the JDK. Click OK. It looks like this:

![JAVA_HOME_PATH](https://i.imgur.com/5l9F3ZJ.png).

Now you can test the install. Open Command Prompt and use command 

{% highlight text %}
javac -version
{% endhighlight %}

If you see a version number you did everything right!

#### Install Android SDK Tools with Android Studio

We will install the Android Studio because at the moment it is the best way to quickly and easily install all the most necessary things for Android Development. The list of things includes:

1. Android Development Kit (Android SDK, Android SDK Manager, Android SDK Platform-tools, Android SDK Build-tools)
2. Android Emulator with a large number of Android configurations
3. IDE (for Android Development on Java)
4. Gradle
5. It would be very helpful if you are learning Java, and in the future want to start developing for Android on Java

So, download Android Studio from official web site [developer.android.com](http://developer.android.com/sdk/index.html)

Android Studio Installation is very simple and you just need to follow the instructions. But you should take note on Android SDK Installation Location.

![SDK](https://i.imgur.com/GhxRmG6.png)

After Android Studio installation is complete you need to add new **ANDROID_HOME** system variable with path to your Android SDK.

Open **Control Panel > System and Security > System > Advanced System Settings**. Then in **System Properties** window select the **Advanced tab** and click the **Environment Variables** button. In the list **System Variables** click button **New...** then in **Variable name** field add **ANDROID_HOME** and in **Variable value** field add path to your Android SDK. It looks like this

![ANDROID_HOME](https://i.imgur.com/PBYgny4.png)

Click OK.

Now you need to add Android SDK and Android SDK Tools to PATH System Variable.
In the list **User variables** select **PATH** and click the **Edit** button. At the end of the field **Variable value**, add a semicolon and follow paths:

{% highlight text %}
C:\Users\User\AppData\Local\Android\sdk;C:\Users\User\AppData\Local\Android\sdk\tools;C:\Users\User\AppData\Local\Android\sdk\platform-tools;
{% endhighlight %}

It looks like this

![PATH_SDK_TOOLS](https://i.imgur.com/EJbXNMU.png)

Click OK.

Now you can test the install. Open Command Prompt and use command

{% highlight text %}
adb version
{% endhighlight %}

This should display the version of the Android Debug Bridge.
If you see a version number you did everything right!

Now again open Command Prompt and use command

{% highlight text %}
android
{% endhighlight %}

for open Android SDK Manager.

![ANDROID_SDK_MANAGER](https://i.imgur.com/XVXU5K5.png)

In the Android SDK Manager select to install

1. Android SDK Tools
2. Android SDK Platform-tools
3. Android SDK Build-tools
4. Android SDK Build-tools
5. Android 6.0 (API 23)
6. Android 5.1.1 (API 22)
7. Android 5.0.1 (API 21)
8. Android 4.2.2 (API 17)
9. GPU Debugging tools
10. Android Support Repository
11. Android Support Library
12. Google Play services
13. Google Repository
14. Google USB Driver
15. Intel x86 Emulator Accelerator (HAXM installer)

and click Install button.

> Some of these components may be already installed

Note:

[Cordova Android Supported API Levels](https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#requirements-and-support)

![Cordova_Android_Supported_API_Levels](https://i.imgur.com/A9FEXrL.png)

[Understanding Android API Levels](https://developer.xamarin.com/guides/android/application_fundamentals/understanding_android_api_levels/)

[Android Platform/API Version Distribution](http://developer.android.com/intl/ru/about/dashboards/index.html)

![API_VERSIONS](https://i.imgur.com/MFcCN4u.png)

#### Install Apache Ant

Apache Ant is a Java library and command-line tool whose mission is to drive processes described in build files as targets and extension points dependent upon each other. The main known usage of Ant is the build of Java applications. Ant supplies a number of built-in tasks allowing to compile, assemble, test and run Java applications.

For install Apache Ant download zip archive from official web site [http://ant.apache.org/bindownload.cgi](http://ant.apache.org/bindownload.cgi) and extract it to any directory.

Now add new **ANT_HOME** system variable. I repeat instructions.

Open **Control Panel > System and Security > System > Advanced System Settings**. Then in **System Properties** window select the **Advanced tab** and click the **Environment Variables** button. In the list **System Variables** click button **New...** then in **Variable name** field add **ANT_HOME** and in **Variable value** field add path where you extracted Apache Ant. It looks like this

![ANT_HOME](https://i.imgur.com/rrrcZIJ.png)

Click OK.

Then add path to the bin directory of the Apache Ant to **PATH** System Variable.

In the list **User variables** select **PATH** and click the **Edit** button. At the end of the field **Variable value**, add a semicolon and follow path:

{% highlight text %}
C:\Users\User\apache-ant-1.9.6\bin
{% endhighlight %}

It looks like this

![PATH_ANT_HOME](https://i.imgur.com/7ClJiEP.png)

Click OK.

#### Install Git

Install git with default settings from official web site [https://git-scm.com/](https://git-scm.com/) for Windows. And add path to the bin directory of the Git to PATH system variable.

#### Install Cordova and Phonegap

After installing all the necessary components to get started with Apache Cordova on Windows, we finally can start to install [Apache Cordova](https://www.npmjs.com/package/cordova).

Open Command Prompt and install Cordova using command

{% highlight text %}
npm install -g cordova
{% endhighlight %}

and install Phonegap using command

{% highlight text %}
npm install -g phonegap@latest
{% endhighlight %}

###### Basic Usage

Open Command Prompt and start new project

{% highlight text %}
cordova create myApp com.myCompany.myApp myApp
{% endhighlight %}

after creating the project proceed to path of the project

{% highlight text %}
cd myApp
{% endhighlight %}

for add plugins you can use command

{% highlight text %}
cordova plugin add cordova-plugin-name --save
{% endhighlight %}

add Android platform

{% highlight text %}
cordova platform add android --save
{% endhighlight %}

for add a specific android version use **codrova platform add android@version --save** for example

{% highlight text %}
cordova platform add android@4.1.1 --save
{% endhighlight %}

build your APK

{% highlight text %}
cordova build android --verbose
{% endhighlight %}

run your project in emulator

{% highlight text %}
cordova emulate android
{% endhighlight %}

for emulate Android with specify virtual device use key **--target=avdName** where avdName is AVD Name in AVD Manager

{% highlight text %}
cordova emulate --target=avdName android
{% endhighlight %}

> If your CPU is not Intel Be sure to read chapter 'If your CPU is not Intel'

#### Difference between Apache Cordova and Phonegap

In practical terms, the difference between Phonegap and Cordova no. Both are set locally, both are able to pull the plugins from the repository, both have the same bugs, because one based code. The difference looks like this:

{% highlight text %}
phonegap local build android // local - because it is not in the cloud, can be remote

cordova build android // Cordova always local
{% endhighlight %}

I chose Cordova, since:

1. I do not need integration with cloud.
2. Phonegap updated almost simultaneously with Cordova, but not both.
3. I for Open Source

For more details you can read here: [PhoneGap, Cordova, and what’s in a name?](http://phonegap.com/2012/03/19/phonegap-cordova-and-what%E2%80%99s-in-a-name/)

#### If your CPU is not Intel

I will be brief. If your CPU is not Intel, standard Android SDK emulator does not will start with x86 architecture. Since AMD processors do not support virtualization HAXM.

But there is good news. You will still be able to run Android SDK emulator if you create a new configuration of the virtual device with ARM CPU.

![ANDROID_EMULATOR_ARM](https://i.imgur.com/3LKo8HI.png)

To create a new virtual device open Command Prompt and run Android SDK Manager with command **android**. When the manager will be run open **Tools > Manage AVDs...** and click to **Create** button

![OPEN_AVD_MANAGER](https://i.imgur.com/b1P77l5.png)

![AVD_MANAGER](https://i.imgur.com/60rZd6D.png)

Disadvantages of Android SDK Emulator:

1. Very slowly, if you do not use HAXM.
2. No emulation Bluetooth, OTG, headphone and some other parameters [http://developer.android.com/intl/ru/tools/devices/emulator.html#limitations](http://developer.android.com/intl/ru/tools/devices/emulator.html#limitations)

#### Genymotion

Just use Genymotion emulator :) [genymotion.com](https://www.genymotion.com/)
