---
title: Fiddle调试iOS/Android APP SSL安全证书的方法
date: 2016-02-23 15:41:08
tags:
- iOS
- Android
- SSL debug
- Fiddler
---
在调试APP时，Fiddler是个很好用的工具，当调试SSL接口时，需要做一些配置才能绕过SSL证书的限制。

1. 进入菜单：Tools > Fiddler Options > Connections. 勾选：Allow remote computers to connect.
2. 设置iOS或Android的代理，注意IP和端口都要正确
3. 下载 CertMaker for iOS and Android 插件，下载地址：http://www.telerik.com/fiddler/add-ons
4. 重启Fiddler
5. 用iPhone或Android手机打开： http://ipv4.fiddler:8888，如果不能访问请使用ip进行访问
6. 点击最下面的“FiddlerRoot certificate”链接，确认安装证书
7. 再进入Fiddler，就可以查看SSL接口的数据了

* 如果还是无法生效，可以右键点击连接，添加URL到proxy列表。