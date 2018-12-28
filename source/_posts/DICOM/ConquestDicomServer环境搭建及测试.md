---
title: DCMTK + Conquest DICOM Server 环境测试
date: 2018-12-25 14:32:20
categories: DICOM
tags:
toc: true
---

更进一步

<!-- More -->

## Conquest 服务器安装

## DCMTK 测试

命令参数示例：
``` c
echoscu.exe -v -aet DebugTest -aec MGIUSDICOM 192.168.28.128 5678

// 查询检索测试
findscu.exe -v -aet DebugTest -aec MGIUSDICOM -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678

// 上句使用默认查询模型: -W Worklist 其他可选性: -P patient -S study -O patient/study except worklist 
// 是引用 -P 或 -S 或 -O 时需要指定查询等级(0x0008, 0x0052)，否则服务器无法正确处理。
// Patient Root Query/Retrieve          : Patient Level, Study Level, Series Level, Composite Object Instance Level.
// Study Root Query/Retrieve            : Study Level, Series Level, Composite Object Instance Level.
// Patient/Study Only Query/Retrieve    : Patient Level, Study Level.
findscu.exe -v -S -aet DebugTest -aec MGIUSDICOM localhost 5678 -k 0x0010,0x0010="HEAD EXP2" -k 0x0008,0x0052="STUDY"

// 查询指定属性列表
findscu.exe -d -S -aet DebugTest -aec MGIUSDICOM localhost 5678 -k 0010,0010="HEAD EXP2" -k 0008,0052="STUDY" -k 0008,0020="" -k 0020,000d="" -k 0020,0010="" -k 0020,000e=""

// 为何 RadiAnt 能同时检索 STUDY 和 IMAGE ？ tag: (0020,1208) DCM_NumberOfStudyRelatedInstances
findscu.exe -d -S -aet DebugTest -aec MGIUSDICOM localhost 5678 -k PatientName -k 0008,0052="STUDY" -k PatientID -k StudyInstanceUID -k SeriesInstanceUID -k SeriesNumber -k NumberOfStudyRelatedInstances

// Qt Demo 中使用
findscu.exe -d -S -aet DebugTest -aec MGIUSDICOM localhost 5678 -k PatientName="HEAD EXP2" -k "QueryRetrieveLevel=STUDY" -k PatientID -k StudyInstanceUID -k NumberOfStudyRelatedInstances

// 拉取测试
getscu.exe -v +v -aet DebugTest -aec MGIUSDICOM -pdu 32768 -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678

// 存储测试
storescu.exe -v -aet DebugTest -aec MGIUSDICOM 192.168.28.128 5678 ./dicom_package.dcm

// 移动/转存测试
storescp.exe -v +v -pdu 32768 5680
movescu.exe -v -aet DebugTest -aec MGIUSDICOM -aem DebugTest -k 0x0010,0x0010="HEAD EXP2" 192.168.28.128 5678
```

注意事项：
* movescu.exe 测试时需 storescp.exe 配合。
 【C-MOVE 请求发出后，服务端反向建立连接发送 C-STORE 请求】
* PDU`Protocol Data Unit` 小于服务端时，数据传输异常。
 【通过 -pdu 调整最大 pdu】


## 关键知识点

Query/Retrieve 信息模型的分层结构

* Patient Root Query/Retrieve 信息模型
* Study Root Query/Retrieve 信息模型
* Patient/Study Root Query/Retrieve 信息模型

每种信息模型按照分层的形式进行组织，DICOM 标准根据实体-关系模型（Entity-Relation Model）将其分为不同级别，由 Query/Retrieve Level 属性指明所请求的级别。
tag: (0x0008,0x0052)

| 信息模型 | 等级 |
|:--------:|:-----|
| Patient Root Query/Retrieve | `Patient Level` <br> `Study Level` <br> `Series Level` <br> `Composite Object Instance Level` |
| Study Root Query/Retrieve | `Study Level` <br> `Series Level` <br> `Composite Object Instance Level` |
| Patient/Study Root Query/Retrieve | `Patient Level` <br> `Study Level` |

DCMTK 源码中定义的等级：(dcmqrdbi.h)
`PATIENT_LEVEL`
`STUDY_LEVEL`
`SERIES_LEVEL`
`IMAGE_LEVEL`

``` c
enum DB_LEVEL
{
    /// DICOM Q/R patient level
    PATIENT_LEVEL,
    /// DICOM Q/R study level
    STUDY_LEVEL,
    /// DICOM Q/R series level
    SERIE_LEVEL,
    /// DICOM Q/R instance level
    IMAGE_LEVEL
};
```

