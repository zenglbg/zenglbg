---
title: 详解CSS中:nth-child的用法
categories:
  - css
tags:
  - css
---

#详解 CSS 中:nth-child 的用法
前端的哥们想必都接触过 css 中一个神奇的玩意，可以轻松选取你想要的标签并给与修改添加样式，是不是很给力，它就是“:nth-child”。

- `:nth-child(2)`选取第几个标签，“2 可以是你想要的数字”

```
.demo01 li:nth-child(2){background:#090}
```

- `:nth-child(n+4)`选取大于等于 4 标签，“n”表示从整数，下同

```
.demo01 li:nth-child(n+4){background:#090}
```

- `:nth-child(-n+4)`选取小于等于 4 标签

```
.demo01 li:nth-child(-n+4){background:#090}
```

- `:nth-child(2n)`选取偶数标签，2n 也可以是 even

```
.demo01 li:nth-child(2n){background:#090}
```

- `:nth-child(2n-1)`选取奇数标签，2n-1 可以是 odd

```
.demo01 li:nth-child(2n-1){background:#090}
```

- `:nth-child(3n+1)`自定义选取标签，3n+1 表示“隔二取一”

```
.demo01 li:nth-child(3n+1){background:#090}
```

- `:last-child`选取最后一个标签

```
.demo01 li:last-child{background:#090}
```

- `:nth-last-child(3)`选取倒数第几个标签,3 表示选取第 3 个

```
.demo01 li:nth-last-child(3){background:#090}
```
