---
layout: post
title: Xcode9 CocoaPods安装
date: 2017-09-24
categories: blog
tags: [CocoaPods]
description: 讲一下Xcode9安装CocoaPods的方法

---

* 1、安装RVM
* 2、安装Ruby环境
* 3、设置Ruby版本
* 4、设置Ruby源
* 5、安装cocoapods
* 6、cocoapods使用

## 安装RVM
1.先安装Xcode开发工具
就是安装Xcode安装完成后打开然后在命令行执行以下命令
```
$ xcode-select --install
```
2.命令行执行
```
$ curl -L https://get.rvm.io | bash -s stable
```
3.执行下列命令
```
$ source ~/.rvm/scripts/rvm
```
如果你重开命令行的话就不需要这么做
4.检查是否安装正确
```
$ rvm -v
```

## 安装Ruby环境
1.列出已知云端Ruby版本
```
$ rvm list known
```
2.选择ruby版本安装
这里用ruby 2.4.1为例进行安装
```
$ rvm install 2.4.1
```
然后等待漫长的下载，配置，编译，安装操作
3.运行rvm list检查ruby本地版本
```
$ rvm list
$ ruby -v
```

## 设置ruby源
1.查看当前gem source
```
$ gem sources -l
```
2.删除旧源
```
$gem source -r https://rubygems.org/
```
3.添加新的ruby china 源
```
$ gem source -a https://ruby.taobao.org
```

## 安装CocoaPods
执行以下命令安装CocoaPods
```
$ sudo gem install  cocoapods
```

## CocoaPods使用
随便以一种方式新建一个名为Podfile的文件放到你的工程根目录下（不能写成别的名字，也可以自己在工程根目录里面直接新建）

Podfile文件内容的格式应该如下：

platform :ios, '8.0' #(注明你的开发平台以及版本，'8.0'忽略不写即为最新版本)

pod 'AFNetworking', '~> 2.5.3' #('~> 2.5.3'为版本号，忽略不写即为最新版本)

pod 'SDWebImage', '~> 3.7.2'

然后在Terminal进入工程所在的根目录（工程根目录）中执行 ：

pod install

这样，AFNetworking和SDWebImage就已经下载完成并且设置好了编译参数和依赖，以后使用的时候切记如下两点：

1.从此以后需要使用Cocoapods生成的 .xcworkspace文件来打开工程，而不是使用以前的.xcodeproj文件

2.每次更改了Podfile文件，都需要重新执行一次pod update命令

# 补充
文章中的思路借鉴了很多人，感谢他们


