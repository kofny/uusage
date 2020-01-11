---
title: Java
date: 2020-01-11 15:56:12
categories:
	- dev
tags:
	- java
	- usage
	- languages
---

## jdk11

### Install

```bash
# download https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html
mv jdk11.tar.gz /usr/local/share/
sudo vim ~/.zshrc
export JAVA_HOME= # 这里填写你安装的路径
export CLASSPATH =.: ${JAVA_HOME}/lib
export PATH = ${JAVA_HOME}/bin:$PATH
java -version
```

