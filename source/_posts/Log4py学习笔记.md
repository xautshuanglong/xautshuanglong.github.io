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
import logging,logging.config

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
streamFmt = logging.Formatter('[ %(levelname)-8s ] %(asctime)s.%(msecs)d %(name)s %(message)s','%H:%M:%S')
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

logging.shutdown()# 更安全地退出
```
3. 配置文件使用说明 
```bash
	<< Log4pyConfig.py >>

# encoding:utf-8
print("log4py configuration testing")
import logging,logging.config
# 通过配置文件 [log4py.conf] 设置 log4py 的属性
logging.config.fileConfig("log4py.conf")
log = logging.getLogger()
#log1 = logging.getLogger("stream")
#log2 = logging.getLogger("tempFile")
#log3 = logging.getLogger("rotateFile")
log.setLevel(logging.NOTSET)
log.debug("Debug Testing ...")
log.info("Info Testing ...")
log.warn("Warn Testing ...")
log.warning("Warinig Testing ...")
log.error("Error Testing ...")
log.fatal("Fatal Testing ...")
log.critical("Critical Testing ...")
logging.shutdown()

	<< log4py.conf >>

# log4py configuration file

[loggers]
keys=root,stream,tempFile,rotateFile

[handlers]
keys=streamHandler,fileHandler,rotateHandler

[formatters]
keys=streamFmt,rotateFmt

[logger_root]
level=NOTSET
handlers=streamHandler,fileHandler,rotateHandler

[logger_stream]
level=DEBUG
handlers=streamHandler
qualname=stream
propagate=0

[logger_tempFile]
level=DEBUG
handlers=fileHandler
qualname=tempFile
propagate=0

[logger_rotateFile]
level=DEBUG
handlers=rotateHandler
qualname=rotateFile
propagate=0

[handler_streamHandler]
class=StreamHandler
level=DEBUG
formatter=streamFmt
args=(sys.stdout,)

[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=streamFmt
args=('tempFile.log','w',)

[handler_rotateHandler]
class=handlers.RotatingFileHandler
level=INFO
formatter=rotateFmt
args=('rotateFile.log','a',10485760,5,)

[formatter_streamFmt]
format=[ %(levelname)-8s ] %(asctime)s.%(msecs)d %(name)s %(message)s
datefmt=%H:%M:%S

[formatter_rotateFmt]
format=[ %(levelname)-8s ] %(asctime)s %(name)s %(message)s
```
4. 字典配置说明
```bash
	<< Log4pyDictConfig.py >>
# encoding:utf-8

import logging,logging.config

print("log4py dictionary configuration")

LOG_DICT= {
	'version':1,
	'loggers':{
		'root':{
			'level':'DEBUG',
			'handlers':['streamHandler','fileHandler','rotateHandler']
		},
		'stream':{
			'level':'DEBUG',
			'handlers':['streamHandler']
		},
		'tempFile':{
			'level':'DEBUG',
			'handlers':['fileHandler']
		},
		'rotateFile':{
			'level':'INFO',
			'handlers':['rotateHandler']
		}
	},
	'handlers':{
		'streamHandler':{
			'class':'logging.StreamHandler',
			'level':'DEBUG',
			'formatter':'streamFmt',
			'stream':'ext://sys.stdout'
		},
		'fileHandler':{
			'class':'logging.FileHandler',
			'level':'DEBUG',
			'formatter':'streamFmt',
			'filename':'tempFile.log',
			'mode':'w'
		},
		'rotateHandler':{
			'class':'logging.handlers.RotatingFileHandler',
			'level':'INFO',
			'formatter':'rotateFmt',
			'filename':'rotateFile.log',
			'mode':'a',
			'maxBytes':10485760,
			'backupCount':5
		}
	},
	'formatters':{
		'streamFmt':{
			'format':'[ %(levelname)-8s ] %(asctime)s.%(msecs)d %(module)s %(funcName)s %(message)s',
			'datefmt':'%H:%M:%S'
		},
		'rotateFmt':{
			'format':'[ %(levelname)-8s ] %(asctime)s %(module)s %(funcName)s %(message)s'
		}
	}
}

logging.config.dictConfig(LOG_DICT)
log = logging.getLogger("root")

log.debug("Debug Testing ...")
log.info("Info Testing ...")
log.warn("Warn Testing ...")
log.warning("Warinig Testing ...")
log.error("Error Testing ...")
log.fatal("Fatal Testing ...")
log.critical("Critical Testing ...")

logging.shutdown()
```

## Python 日志模块 logging 简介
logging分为4模块：loggers,handlers,filters,and formatters.
* loggers: 提供应用程序调用的接口
* handlers: 把日志发送到指定的位置
* filters: 过滤日志信息
* formatters: 格式化输出日志
### Logger
		Logger.setLevel()       # 设置日志级别
		Logger.addHandler()     # 增加日志处理器
		Logger.removeHandler()  # 删除日志处理器
		Logger.addFilter()      # 增加过滤器
		Logger.removeFilter()   # 删除过滤器
		# 创建不同级别的日志
		Logger.debug()    # DEBUG
		Logger.info()     # INFO
		Logger.warn()     # WARNING
		Logger.warning()  # WARNING
		Logger.error()    # ERROR
		Logger.fatal()    # CRITICAL
		Logger.critical() # CRITICAL
		getLogger()获取日志的根实例
### Handler
		Handler.setLevel()     # 设置日志级别
		Handler.setFormatter() # 设置输出格式
		Handler.addFilter()    # 增加过滤器
		Handler.removeFilter() # 删除过滤器
### Formatter
时间默认形式：%Y-%m-%d %H:%M:%S
格式为：%()s

|格式|说明|
|:---:|:---:|
|%(levelno)s|打印日志级别的数值|
|%(levelname)s|打印日志级别的名称|
|%(pathname)s|打印当前执行程序的路径==sys.argv[0]|
|%(filename)s|打印当前执行程序名|
|%(funcName)s|打印当前函数名|
|%(lineno)d|打印当前行号|
|%(asctime)s|打印当前时间（升序）|
|%(created)f|打印当前时间（浮点型，不易读）|
|%(thread)d|打印当前线程ID|
|%(threadName)s|打印当前线程名称|
|%(process)d|打印当前进程ID|
|%(processName)s|打印当前进程名称|
|%(message)s|打印日志信息|
|%(module)s|打印模块名称|
|%(msecs)d|打印当前时间毫秒数|
|%(name)s|打印 logger 的名称|
|%(relativeCreated)d|打印相对于模块加载完成是的毫秒数|

## Python中的日志级别
小于指定级别的日志消息将被忽略。
Logging messages which are less severe than specified level will be ignored.

| Level | Numeric value ||
|:---:|:-----:|:---:|
|CRITICAL|50|临界|
|ERROR|40|错误|
|WARNING|30|警告|
|INFO|20|通知|
|DEBUG|10|调试|
|NOTSET|0|未置|

### 参考网址
http://www.phperz.com/article/16/0120/93998.html
http://blog.csdn.net/fxjtoday/article/details/6307285
http://zeping.blog.51cto.com/6140112/1259065
http://blog.csdn.net/chosen0ne/article/details/7319306

[官方文档](https://docs.python.org/2.7/library/logging.html#module-logging "Logging facility for Python")

## 其它相关网址

[官方文档](https://docs.python.org/2.7/library/logging.html#module-logging "Logging facility for Python")

## 其它相关网址
[Python时间格式化](http://www.runoob.com/python/python-date-time.html "http://www.runoob.com/")
