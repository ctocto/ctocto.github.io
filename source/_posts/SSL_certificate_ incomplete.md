---
title: 解决SSL证书链不完整
date: 2021-12-22 19:25:00
tags: [SSL,证书,CA]
categories: 饭碗(技术)
---

使用不完整的证书，当用户访问防护域名对应的浏览器时，因不受信任而不能正常访问防护域名对应的浏览器。

### 解决
将证书链（如：根证书->中级证书->自己的证书）的PEM编码倒序组合，之后在重新配置就可以了。如下图：

<img src="https://raw.githubusercontent.com/ctocto/ctocto.github.io/develop/images/zh-cn_image_0000001170698116.png" alt="证书" />

### 不知道上级证书的PEM编码？
请参考 [如何解决SSL证书链不完整？](https://support.huaweicloud.com/scm_faq/scm_01_0217.html) 下载证书



----

> [如何解决SSL证书链不完整？](https://support.huaweicloud.com/scm_faq/scm_01_0217.html) 
> 
> [SSL证书文件校验工具](https://www.chinassl.net/ssltools/decoder-ssl.html) 
> 
> [SSL状态检测](https://myssl.com/ssl.html) 
> 
> [KeyManager - 一站式证书申请和证书密钥管理](https://keymanager.org/)
