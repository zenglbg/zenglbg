---
title: HTML细线表格
categories:
  - flutter
tags:
  - flutter
---

这段代码定义了 border = 1px 的表格，但实际上表格的实际边框宽度为 2px, 这是因为表格边框由：表格外边框和单元格边框两部分构成。

```

<table border="1" cellspacing="0" bordercolor="#000000" width = "80%">
    <tr>
        <td>1.1</td>
        <td>1.2</td>
    </tr>
    <tr>
        <td>2.1</td>
        <td>2.2</td>
    </tr>
<table>
```

定义一个细线表格(实际边宽为 1px)

- cellspacing 和背景色技术

```
<table border="0"
       cellspacing="1"
       bgcolor="#000000"
       width = "80%"
>
    <tr bgcolor="#ffffff">
        <td>1.1</td>
        <td>1.2</td>
    </tr>
    <tr bgcolor="#ffffff">
        <td>2.1</td>
        <td>2.2</td>
    </tr>
<table>
```

- 使用 border-collapse 属性

```
<table border="1"
       cellspacing="0"
       bordercolor="#000000"
       width = "80%"
       style="border-collapse:collapse;"
>
    <tr>
        <td>1.1</td>
        <td>1.2</td>
    </tr>
    <tr>
        <td>2.1</td>
        <td>2.2</td>
    </tr>
<table>
```
