---
title: DCMTK + Conquest DICOM Server 环境测试
date: 2018-12-25 14:32:20
categories: DICOM
tags:
toc: true
---

## Conquest 服务器安装

## DCMTK 测试

命令参数示例：
``` c
echoscu.exe -v -aet DebugTest -aec MGIUSDICOM 192.168.28.128 5678
findscu.exe -v -aet DebugTest -aec MGIUSDICOM -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678
getscu.exe -v +v -aet DebugTest -aec MGIUSDICOM -pdu 32768 -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678
storescu.exe -v -aet DebugTest -aec MGIUSDICOM 192.168.28.128 5678 ./dicom_package.dcm
storescp.exe -v +v -pdu 32768 5680
movescu.exe -v -aet DebugTest -aec MGIUSDICOM -aem DebugTest -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678
```

注意事项：
* movescu.exe 测试时需 storescp.exe 配合。
 【C-MOVE 请求发出后，服务端反向建立连接发送 C-STORE 请求】
* PDU`Protocol Data Unit` 小于服务端时，数据传输异常。
 【通过 -pdu 调整最大 pdu】

