---
title: flutter 组件化开发，抽离公共ui
categories:
  - flutter
tags:
  - flutter
---

# flutter 组件化开发，抽离公共 ui

初学 flutter 会把一个页面的所有代码都写在一个文件中，这样的代码
<img src="https://pic.zenglbg.com/images/flutter/2021-07-28 22.25.38.jpg" alt="示例图片" style="zoom:75%;" />

## 抽离 appbar

> AppBar 和普通的 container 组件不太一样，会提示下面这种类似的错误
> `error: The argument type 'xxx' cannot be assigned to the parameter type 'PreferredSizeWidget'. (argument_type_not_assignable at [so_app] lib/xxxx.dart:xxx) `
> 我们需继承 PreferredSizeWidget,并实现 preferredSize

```dart
import 'package:flutter/material.dart';

class HomeHeader extends StatelessWidget with PreferredSizeWidget {
  // const HomeHeader({Key? key}) : super(key: key);

  // 抽离appbar必须实现PreferredSizeWidget
  @override
  Size get preferredSize => new Size.fromHeight(46);

  @override
  Widget build(BuildContext context) {
    return AppBar(
      title: Text('首页'),
      backgroundColor: Color(int.parse('2c375b', radix: 16)).withAlpha(255),
      centerTitle: true,
      elevation: 0,
      actions: [
        Padding(
          padding: EdgeInsets.only(right: 20),
          child: Image(
            image: AssetImage('images/icon-notice.png'),
            width: 22,
          ),
        ),
      ],
    );
  }
}
```

## 抽离头部和底部相同的代码为 layout 组件

> 我们很大概率会遇到 header 和 footer 固定，内容变化的页面。这时候可以把这部分代码抽离出来。

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  static const String _title = 'Flutter Code Sample';

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _title,
      home: Scaffold(
        appBar: AppBar(title: const Text(_title)),
        body: Row(
          children:[
            testContainer(),
            testContainer(child: Container(
              color: Colors.teal,
              height: 50,
              child: Text('test child')
            ))
          ]
        ),
      ),
    );
  }

  Widget testContainer({Widget child = const Text('default child')}){
    return Container(
    margin: const EdgeInsets.all(10),
      child:Column(
      children:[
        Container(
          width:100,
          height:50,
          color: Colors.red
        ),
        Flexible(
          child: Container(
            margin: const EdgeInsets.all(10),
            child: child
          ),
        ),
        Container(
          width:100,
          height:50,
          color: Colors.green
        )
      ]
    ));
  }
}
```

<img src="https://pic.zenglbg.com/images/flutter/2021-07-28 22.47.11.jpg" alt="抽离头部和底部相同的代码为layout组件示意图" style="zoom:75%;" />
