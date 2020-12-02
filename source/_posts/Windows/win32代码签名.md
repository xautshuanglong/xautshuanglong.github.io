---
title: Windows环境下代码签名
date: 2020-11-27 17:12:45
categories: Windows
tags: signature
toc: true
---

SPC — 软件发布者证书

<!-- More -->

## 实战演练

### 方式一
``` bash
makecert -r -pe -n "CN=Code Signature CA" -ss CA -sr CurrentUser -a sha256 -cy authority -sky signature -eku 1.3.6.1.5.5.7.3.3 -sv CodeSignatureCA.pvk CodeSignatureCA.cer
// 此处选择手动安装于 本地计算机 受信任的根证书颁发机构
certutil -user -addstore Root code_signature_ca.cer
makecert -pe -n "CN=Code Signature" -a sha256 -cy end -sky signature -ic CodeSignatureCA.cer -iv CodeSignatureCA.pvk -sv CodeSignatureSPC.pvk CodeSignatureSPC.cer
pvk2pfx -pvk code_signature.pvk -po password -spc code_signature.cer -pfx code_signature.pfx
// password 在生成 pfx 文件时指定
// 时间戳用于设置当前签名时间以及校验是否在证书可用时间范围内
signtool sign /v /fd SHA256 /p password /f CodeSignatureSPC.pfx /t http://timestamp.globalsign.com/scripts/timstamp.dll Setup.msi
```

### 方式二
``` bash
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")

Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, Thumbprint

// Thumbprint 根据 Get-ChildItem 返回的指纹来定
// Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $password
$password = ConvertTo-SecureString -String password -Force -AsPlainText
Export-PfxCertificate -cert "Cert:\CurrentUser\My\E19B89242AA47DA6F8640AC98F7AE4DA62E147F5" -FilePath E:\\test.pfx -Password $password
```

### 证书安装
代码签名证书需要被安装到`受信任的根证书颁发机构`才能接受被该证书签名的软件或脚本。
方式一：生成的证书本来就是文件形式，在目标机器上双击安装即可。
方式二：生成的证书是存到系统中的，需要导出成证书文件后安装到目标机器上。
安装步骤：双击证书文件(\*.cer) -> 安装证书 -> 当前用户/本地计算机 -> 将所有的证书都放入下列存储 浏览 -> 受信任的根证书颁发机构 -> 确定

## 参考网址
[如何在Windows上为代码签名创建自签名证书](https://blog.csdn.net/weixin_43982401/article/details/104437205)
[为程序包签名创建证书](https://docs.microsoft.com/zh-cn/windows/msix/package/create-certificate-package-signing)
[受信任的根证书颁发机构证书存储](https://docs.microsoft.com/zh-cn/windows-hardware/drivers/install/trusted-root-certification-authorities-certificate-store)
[SignTool.exe（签名工具）](https://docs.microsoft.com/zh-cn/previous-versions/dotnet/netframework-4.0/8s9b9yaz&#40;v=vs.100&#41;)
[MakeCert](https://docs.microsoft.com/en-us/windows/win32/seccrypto/makecert)

