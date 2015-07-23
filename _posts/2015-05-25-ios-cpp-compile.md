---
layout: post
title:  "iOS下使用C++编译出来的静态库"
date:   2015-03-23 14:06:05
categories: iOS
excerpt: 
---

* content
{:toc}

做了一个库，在iOS下以静态库.a的形式提供给客户，有一些客户在link时出现找不到std::string等symbol错误。

这是由于该库内部使用到了C++，解决方法为“Build Settings” -> "Apple LLVM 6.0 - Language - C++" -> "C++ Standard Library"选择"libstdc++ (GNU C++ standard library)"

这时候，仍然有可能会出现c++ link错误，可以把一个.m文件改为.mm扩展名，强制编译器最后以c++来进行link