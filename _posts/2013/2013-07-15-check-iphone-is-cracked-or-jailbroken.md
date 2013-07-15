---
layout:post
title:ios检测设备是否越狱和应用是否被破解
categories:
- ios开发
tags:
- iOS
---

某些时候和场景下，你可能需要检测一下当前的设备是否已经越狱或者你的app是否是从官方的appstore下载,而不是从某些国内市场下载过来的。

github上已经有了这样的工具类，可以参照：[https://github.com/itruf/crackify](https://github.com/itruf/crackify)，具体使用方式:
{% highlight objc %}
[Crackify isJailbroken] //检测当前设备是否越狱
[Crackify isCracked] //检测当前应用是否从appstore下载

####为防止被其它市场或工具绕过检测，记得修改类名和方法名。