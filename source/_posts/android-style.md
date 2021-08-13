---
title: android环境配置
categories:
  - android
tags:
  - android
image: https://pic.zenglbg.com/images/girl/girl3/photo_2021-08-06 14.26.47.jpeg
cover: https://pic.zenglbg.com/images/girl/girl3/photo_2021-08-06 14.26.47.jpeg

---

# android 环境配置

## windows

SDK 默认是安装在下面的目录：
![图片](https://pic.zenglbg.com/images/flutter/android_home.png)

```
C:\Users\你的用户名\AppData\Local\Android\Sdk
```

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

## macos

```
brew install adoptopenjdk/openjdk/adoptopenjdk8
```

```
# 如果你不是通过Android Studio安装的sdk，则其路径可能不同，请自行确定清楚
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```
