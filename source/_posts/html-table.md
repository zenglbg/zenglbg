---
title: HTML细线表格
---

这段代码定义了border = 1px的表格，但实际上表格的实际边框宽度为2px, 这是因为表格边框由：表格外边框和单元格边框两部分构成。

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


定义一个细线表格(实际边宽为1px) 
- cellspacing和背景色技术
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

- 使用border-collapse属性
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



