---
title: Vim
date: 2020-01-11 15:56:46
categories:
	- dev
tags:
	- usage
	- editor
---

## 文本替换

### 普通替换

%s 表示全文，g 表示全部替换

```shell
:[range]s/[from]/[to]/[flags]
```

<!-- more -->

#### 举例

全文替换 from 到 to

```shell
:%s/from/to/g
```

### 正则替换写法

使用  \v 后之后的内容就不需要人为转义了。

```shell scrpt
:%s/\v(\W)/\\\1/g # 对所有的非字母、数字字符添加 \
```

## 搜索

查找当前单词：`shift + *`，使用 `n` 查找下一个，使用 `N` 查找上一个

gd 也可以达到类似效果