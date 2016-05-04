---
title: Log4py 学习笔记
date: 2016-05-03 18:12:46
categories: Python
tags: [log4py,Python]
---

### Log4py 学习笔记
1. 小例子
```bash
# encoding:utf-8
print("log4py testing")
import logging
import logging.config

logging.basicConfig(level=logging.DEBUG)# 默认级别：WARNING,WARN,30

log = logging.getLogger()
log.debug("Debug Testing ...")
log.info("Info Testing ...")
log.warn("Warn Testing ...")
log.warning("Warinig Testing ...")
log.error("Error Testing ...")
log.fatal("Fatal Testing ...")
log.critical("Critical Testing ...")
```
2. 进阶例子
```bash
# encoding:utf-8
print("log4py testing")
import logging
import logging.handlers
import sys

# StreamHandler == ConsoleAppender,输出到流，控制台调试
streamFmt = logging.Formatter('[ %(levelname)-8s ] %(asctime)s %(name)s %(message)s','%H:%M:%S')
stream = logging.StreamHandler(sys.stdout)
stream.setLevel(logging.DEBUG)
stream.setFormatter(streamFmt);

# FileHandler == FileAppender,输出到指定文件 [不追加，用于临时运行调试]
tempFile = logging.FileHandler("tempFile.log",'w')
tempFile.setLevel(logging.DEBUG)
tempFile.setFormatter(streamFmt)

# RotatingFileHandler = RollingFileAppender,按指定大小回滚 [追加，长期运行记录]
fileFmt = logging.Formatter('[ %(levelname)-8s ] %(asctime)s %(name)s %(message)s')# 使用默认时间格式
rotateFile = logging.handlers.RotatingFileHandler("rotateFile.log",'a',10485760,5)# 超过 10M 则分割文件，最多保留5个分割文件
rotateFile.setLevel(logging.INFO)
rotateFile.setFormatter(fileFmt)

log = logging.getLogger()
log.setLevel(logging.NOTSET)# 默认是 WARNING 级别，并且被其他 Handler 继承

log.addHandler(stream)
log.addHandler(tempFile)
log.addHandler(rotateFile)

log.debug("Debug Testing ...")
log.info("Info Testing ...")
log.warn("Warn Testing ...")
log.warning("Warinig Testing ...")
log.error("Error Testing ...")
log.fatal("Fatal Testing ...")
log.critical("Critical Testing ...")
```
3. 配置文件使用说明 

## Python中的日志级别
小于指定级别的日志消息将被忽略。
Logging messages which are less severe than specified level will be ignored.

| Level | Numeric value |
|:---:|:-----:|
|CRITICAL|50|
|ERROR|40|
|WARNING|30|
|INFO|20|
|DEBUG|10|
|NOTSET|0|

[官方文档](https://docs.python.org/2.7/library/logging.html#module-logging "Logging facility for Python")

## 其它相关网址

[官方文档](https://docs.python.org/2.7/library/logging.html#module-logging "Logging facility for Python")

## 其它相关网址
[Python时间格式化](http://www.runoob.com/python/python-date-time.html "http://www.runoob.com/")
