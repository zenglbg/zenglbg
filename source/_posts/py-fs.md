---
title: python 目录操作
categories:
  - python
tags:
  - python
image: https://pic.zenglbg.com/images/girl/girl3/photo_2021-08-06 14.26.51.jpeg


---

将 python 的文件系统、路径操作 API 详细的学习了一下，代码都是测试过的，也许很简单，但为了打好基础，还是要有点一丝不苟的精神，希望对你有用……

- 获取当前目录

```
import sys
print(sys.path[0])
```

- 获取命令执行目录

```
import os
print(os.getcwd())
```

- 路径拼接

```
import os
print(os.path.join(os.getcwd(), 'config.ini'))

# /Users/zenglbg/python/config.ini
```

- 路径拆分

```
### os.path.split 直接拆分路径
import os
os.path.split(os.path.join(os.getcwd(), 'config.ini'))
# ('/Users/zenglbg/python/', 'config.ini')

### os.path.splitext 拆分文件后缀
os.path.(os.path.join(os.getcwd(), 'config.ini'))
# ('/Users/zenglbg/python/config', '.ini')
```

> 这些合并、拆分路径的函数并不会检测目录和文件是否真实存在，他们仅仅是对字符串进行操作。

- 判断目录是否存在

```
import os
os.path.exists(directory) #如果目录不存在就返回False
```
