---
title: Git实用技巧
date: 2018-11-15 11:05:20
categories: CommonSense
feature: /images/CommonSense/Git_logo.png
tags: git
---

## 中文显示问题
git status 列举文件名含有中文时，无法正常显示：(文件名编码格式 与 git diff 不一致)
git config \-\-global core.guotepath false
