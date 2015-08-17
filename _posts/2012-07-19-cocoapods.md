---
layout: post
title:  "使用CocoaPods管理第三方库"
date:   2013-02-15 14:06:05
categories: iOS
---

* content
{:toc}

#### CocoaPods安装

使用淘宝提供的源

    gem sources –remove https://rubygems.org/
    gem sources –a http://ruby.taobao.org/

更新RubyGems

    sudo gem update --system

安装CocoaPods

    sudo gem install cocoapods
    
接着

    pod setup
    
这一步比较费时间,会把CocoaPods Specs repository通过git更新到~/.cocoapods/

#### CocoaPods使用

在Xcode项目目录下,使用pod init生成一个缺省Podfile,或者直接建立一个空文件Podfile,在此基础上修改,Podfile示例
    
    platform:ios,’6.1’
    pod "GCDWebServer/WebUploader", "~> 3.0"

保存后执行

    pod install
    
安装库,以后就可以通过xcworkspace文件来打开xcode项目

另外可以使用pod update来更新第三方库
