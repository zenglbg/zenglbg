---
title: android studio Preview 在m1芯片的mac上无法打开终端
tags: [flutter, dart]
categories:
  - flutter
comments:
  - true
keywords: zenglbg, flutter, dart
image: https://pic.zenglbg.com/images/girl/girl2/photo_2021-07-28 18.55.21 (1).jpeg
---

# android studio Preview 在m1芯片的mac上无法打开终端


## 问题

android studio Preview 在m1芯片的mac上打开终端报错，错误显示：
```shell

Cannot open Local Terminal
Failed to start [/bin/bash, --rcfile, /Applications/Android Studio.app/Contents/plugins/terminal/jediterm-bash.in, -i] in /Users/{UserName}/Android
See your idea.log (Help | Show Log in Finder) for the details.

```

## 处理

```shell
git clone https://github.com/JetBrains/pty4j.git
cd pty4j/native
clang -fPIC -c *.c
clang -shared -o libpty.dylib *.o
cp libpty.dylib "/Applications/Android Studio Preview.app/Contents/lib/pty4j-native/darwin/"

```