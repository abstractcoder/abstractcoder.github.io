---
title: How to Get Started Building Mobile Apps With Meteor
layout: post
---

[Meteor](https://www.meteor.com/) [recently announced](https://www.youtube.com/watch?v=zzNoXbv1DX4&feature=youtu.be&t=23m54s) the ability to create mobile apps for iOS and Android using [Cordova](http://cordova.apache.org/). This short guide will show you how to get up and running quickly.

To build an app for iOS first you will need to [install the latest version of Xcode](https://developer.apple.com/xcode/downloads/). 

If you want to build an app for Android you will need to [install the Android SDK](https://developer.android.com/sdk/installing/index.html).

If you don't have Meteor installed first open a terminal window and run

```bash 
curl https://install.meteor.com | /bin/sh
```

Create a project called MobileDemo in the current directory

```bash 
meteor create MobileDemo
cd MobileDemo
```

Upgrade the Meteor project to use the Cordova Preview build

```bash
meteor update --release CORDOVA-PREVIEW@3
```

The add the package required to build an iOS app

```bash
meteor add-platform ios
```

Finally run the app on the iOS simulator

```bash
meteor run ios
```

To build the app for Android you will first need to install the Android Platform (which is currently about 322MB)

```bash
meteor add-platform android
```

Then you can run the app in the Android Emulator

```bash
meteor run android
```

If you have any questions or feedback please share in the comment!