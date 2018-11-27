---
title: StarUML 使用教程
date: 2018-11-27 13:24:58
categories: UML
feature: /images/UML/StarUML_logo.png
tags: uml
toc: true
---

可视化编程

<!-- More -->

## 科学使用方法

StarUML 是用 nodejs 写的（[Electron框架](https://github.com/electron/electron)），新版本中源代码采用 asar 工具打包，无法直接看到 `LicenseManagerDomain.js`。

1. 安装 asar
```
    npm install -g asar
```
1. 解压 app.asar
```
    // 找到 StartUML 安装目录下 resources/app.asar， 默认安装路径为 C:\Program Files\StarUML
    app extract app.asar
```
1. 修改 JavaScript 源码
```js
  // 修改前
  checkLicenseValidity () {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, false)
      UnregisteredDialog.showDialog()
    })
  }

  // 修改后
  checkLicenseValidity () {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      //setStatus(this, false)
      //UnregisteredDialog.showDialog()
      setStatus(this, true)
    })
  }
```

1. 打包 app.asar
```
    asar pack app app.asar
```

