---
layout: post
title: CocoaPods使用
categories:
- ios开发
tags:
- iOS
---

##CocoaPods使用
###简介
用来管理ios项目中的第三方依赖包，类似于mvn的依赖管理工具，不用手工的在xcode项目中增加依赖包。

###官网
[cocoapods.org](http://www.cocoapods.org)

###如何使用
需要先安装ruby和ruby的gem包管理器，或者在xcode中安装命令行工具。
#####安装ruby
[rvm.io](http://rvm.io)
#####使用xcode安装命令行工具
Xcode->Preferences->Downloads->Components点击install按纽

#####安装CocoaPods
    gem install cocoapods
	pod setup
#####更新CocoaPods
    gem update cocoapods
#####常用命令
- 查找版本库中是否有对应的包: `pod search json`

#####在ios项目中使用
- 在项目的根目录，新增 **Podfile** 文件
- 在Podfile文件中设置项目的平台和依赖的包 	
- 运行 `pod install` 自动安装相关的依赖包
- 打开cocoapods生成的xxx.xcworkspace文件来打开项目

Podfile内容示例

	platform:ios                #标识项目运行的平台
    pod 'JSONKit', '~>1.4'      #依赖的第三方包，第二个参数为版本号
    pod 'SVProgressHUD',    :git => 'https://github.com/samvermette/SVProgressHUD.git'   
	pod 'Objection', :head   #加载最新的spec版本
	pod 'AFNetworking', :path => 'libs/AFNetworking'  #本地加载
	pod 'JSONKit', :podspec => 'https://raw.github.com/gist/1346394/1d26570f68ca27377a27430c65841a0880395d72/JSONKit.podspec' #指定spec加载

CocoaPods的默认spec仓储地址：[Specs](https://github.com/CocoaPods/Specs)

#####增加一个pod spec

如果CocoaPods的spec仓储中没有想要的spec,或者自已想定义一个spec,可以使用 pod spec create来创建一个pod spec.

创建的spec文件是基于ruby的DSL，示例如下：

    Pod::Spec.new do |s|
      s.name = 'Demo'
      s.version  = '0.0.1'
      s.license  =  :type ='BSD' 
      s.homepage = 'https://github.com/cdredfox/demo'
      s.authors  =  'Fei Yang' ='cdredfox@gmail.com' 
      s.summary  = 'test.'
      s.source   =  :git ='https://github.com/cdredfox/demo.git', :tag ='v0.0.1' 
      s.source_files = 'Demo.h,m'
      s.framework= 'SystemConfiguration'
      s.requires_arc = true
    end

#####原理
CocoaPods实际上会生成一个名为 project_name.xcworkspace的项目文件，这个文件里面包含了两个工程，一个是主工程，一个是pod的工程，CocoaPods会将所有的依赖都都放到这个名为Pods的工程中，然后再让主工程来依赖这个pods工程，这个Pods工程最终会编译成一个叫libPods.a的文件，主工程只需依赖这个.a文件就可以了。Cocoapods还提供了一个Pods-resourcers.sh脚本，这个脚本每次在**项目编译**的时候都会自动运行这个脚本，将第三方库中的一些资源文件复制到目标目录中。CocoaPod还通过一个名为Pods.xcconfig的文件来在编译时设置所有的依赖和参数。