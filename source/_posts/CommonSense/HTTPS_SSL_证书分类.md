---
title: HTTPS SSL 证书分类
date: 2019-07-01 20:36:36
categories: CommonSense
tags: https
toc: true
---

<!-- More -->

## 数字证书颁发机构
数字证书认证机构 `Certificate Authority，缩写为CA`，是负责发放和管理数字证书的权威机构，并作为电子商务交易中受信任的第三方，承担公钥体系中公钥的合法性检验的责任。

## SSL 证书
SSL 证书是由受信任的数字证书颁发机构 CA，在验证服务器身份后颁发，且具有服务器身份验证和数据传输加密功能。

## SSL 证书级别
SSL 证书根据验证级别，分为三种类型：
* 域名型 SSL 证书，简称 DVSSL `Domain Validation SSL Certificate`
* 企业型 SSL 证书，简称 OVSSL `Organization Validation SSL Certificate`
* 增强型 SSL 证书，简称 EVSSL `Extended Validation SSL Certificate`

## SSL 证书域名类型
根据保护域名的数量，SSL 证书又分为：
* 单域名版：只保护一个具体域名
* 多域名版：可以保护多个具体域名
* 通配符版：可以保护同一个主域名下同一级的所有子域名，不限个数

## OpenSSL
[参考](/2019/02/22/Libraries/OpenSSL学习笔记/ "OpenSSL 学习笔记")

