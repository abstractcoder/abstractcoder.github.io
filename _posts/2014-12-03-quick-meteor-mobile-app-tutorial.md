---
title: Quick Meteor Mobile App Tutorial
layout: post
---

![Meteor Todo App Screenshots](/assets/meteor-todo-app.png)

[Meteor recently released version 1.0](https://www.meteor.com/blog/2014/10/28/meteor-1-0) so I decided to revisit deploying Meteor apps to iOS and Android. The last article showed [how to deploy the default Meteor application](/2014/08/22/how-to-get-started-building-mobile-apps-with-meteor-js.html), but this time I will be deploying the example todo app.

If you don't have Meteor installed first open a terminal window and run

```bash 
curl https://install.meteor.com | /bin/sh
```

If you already have Meteor installed ensure that you are using the latest version by running

```bash
meteor update
```

Next create a new project using the todo example and change to the new project directory

```bash 
meteor create --example todo
cd todo
```

To build an app for iOS first you will need to [install the latest version of Xcode](https://developer.apple.com/xcode/downloads/). 

Then add the package required to build an iOS app

```bash
meteor add-platform ios
```

Finally run the app on the iOS simulator

```bash
meteor run ios
```

The setup for building Android apps is a bit more involved. First you will need to install the Android SDK by running the command

```bash
meteor install-sdk android
```

This command will [walk you through the steps necessary to get your environment setup properly](https://github.com/meteor/meteor/wiki/Mobile-Dev-Install:-Android-on-Mac).

Then add the platform by running 

```bash
meteor add-platform android
```

Finally you can run the app in the Android Emulator

```bash
meteor run android
```

If you have any questions or feedback please share in the comment!