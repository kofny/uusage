---
title: Python
date: 2020-01-11 15:56:24
categories:
	- dev
tags:
	- python
	- usage
	- languages
---

## *.py run as executable

```bash
whereis python3  # find python intepreter
```

add the following code to the first line of *.py

```python
#!/usr/bin/python3
```

```bash
chmod u+x [filename].py  # add(+) {executable} permission(x) to current user(u)
```

<!-- more -->

## Python venv 虚拟环境

```bash
# apt-get install python3.x-venv first
python3 -m venv /path/to/new/virtual/environment  # 创建
source venv/bin/active # 激活
deactivate # 退出
```

## f-string

f-string 的大括号内不能出现 `\` 字面量，如果一定要用，就声明一个变量，如 `right_splash = '\'`

## Error

### pip 升级后报错

**报错内容**

```bash
Traceback (most recent call last):
File "/usr/bin/pip", line 9, in <module>
from pip import main
```

**修改**

```bash
which pip3
```

```python
from pip import __main__  # changed
if __name__ == '__main__':
    sys.exit(__main__._main())  # changed
```

### zipfile.BadZipFile: File is not a zip file

出现这个问题时，定位到报错地点，查看是哪个文件“is not a zip file”，看是不是文件不存在或已损坏