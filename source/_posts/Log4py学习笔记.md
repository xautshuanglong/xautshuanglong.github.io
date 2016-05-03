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
2. 配置文件使用说明 
