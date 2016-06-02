---
layout: page
title: Peiwei's Resume
tags: [Peiwei, Resume]
modified: 2016-06-01T19:05:07.573882-19:00
comments: true
image:
  feature: peiwei_surfing1.jpg
  credit: peiwei
---


> 目前我正积极地寻求Remote的工作，或者外包项目 :]


## 简介	

* 陈培伟，主导过四个iOS App开发
* 92年出生，男，3年开发经验

## 教育经历

* 深圳大学 ,本科 ,2011～2015 ,计算机科学
* Tennessee University At Knoxville ,交换生 ,2014年 ,计算机科学

## 项目经验:


### 1.图片社区类App：

* 主导开发，技术架构是Objective-C+MVVM+ReactiveCocoa+FMDB+AFNetworking+Mantle+SDWebImage+Masonry+Openshare
* 技术点：
* MVVM+ReactiveCocoa 实现数据在多层同步
* 大量的Custom UI framwork,例如做了一套图片选择和文字输入的框架，类似于QQ聊天的输入界面.
* 大量使用Xib,开发速度快，但是维护起来有点难
* 移除shareSDK和友盟的第三平台分享SDK,接入Openshare,为app减重2MB.

### 2.直播类App: 

* 主导开发，技术框架是 Swift+Cocoapods+MVC+Alamofire+MonkeyKing+Realm+SnapKit +RTMP
* 用Realm做数据库操作，使用监听模式，数据库对应的model层改变，更新对应的UI和逻辑操作。
* 用MonkeyKing集合了所有第三方平台获取用户信息的接口，避免使用MOB和友盟的sdk
* 利用Core Graphics 绘制爱心❤️等定制图标 和 Core Animations实现动画。
* 纯代码写UI,为了进入一个专心写程序的世界，弃用Xib.
* RTMP可有效降低延迟,IM准备接入融云的SDK




### 3.LBS App：

* LBS智能景区旅游平台,  技术架构是 Swift+MVC+Mapbox+Alamofire+SQLite
* 技术点：
* 自定义地图在Mapbox上的兼容运行
* 几套坐标体系的转换（Google->Baidu->Custom)
* 坐标的精确度与电池消耗的平衡
* 地图导航路线的绘制




## 技术

> 精通Swift，Objective-C 和 Cocoa Touch
> Github & Git , Google-StackOverflower

#### 关于Swift和Objective-C：
初期参与过Swift和OC的hybrid项目，爬过Swift1.0的坑，后来主导过纯Objective-C的项目，深深感受了Swift和Objective-C的差距，目前新项目全部用Swift2.2写，因为Github上的新framework都是Swift2.2写的，在2016年尾,Swift3.0会发布，期待它的更多支持。

#### 关于JSON解析
个人喜欢直接解析ObjectForKey,而不是使用Mantle等解析库先声明Model内Key-Value的对应关系，这个过程消耗了工程师的时间，而且很多情况下iOS工程师会因为这个问题给服务端提一些死板的硬性要求，我的理解是集成度越高的东西，灵活性更低，我喜欢更高的灵活性。


#### 关于数据库Realm
一个集成度很高的DB,可以直接存取Model,不需要“自己去生成Model,取到数据库的数据再人工赋值给model的property”,这样可以大量减少ViewController的工作和代码，还有一个特性是可以监听某个Model类的变化，便于我们在代码内使用监察者模式。

#### 关于MonkeyKing
(MonkeyKing helps you post messages to Chinese Social Networks, without their buggy SDKs) 这里的their指的是国内集成社会分享的SDK服务,避开了很多bug.


## 目前的一些想法

#### 个人平时也喜欢捣鼓一些Web端的开发：
* 目前正利用python和Flask写一个简单的Blog
* 用Javascript,Jquery,CSS,HTML写过一些简单的管理网站，目前正在准备多学点前端知识

#### 截至6月1号的学习状况：
* 已经自学了如何利用Swift和SpritKit做Flappy Bird游戏，准备做一个中文教程
* 准备将<a markdown="0" href="https://github.com/coolbeet/CBStoreHouseRefreshControl">CBStoreHouseRefreshControl</a>转换成Swift版本，在github开源



## 关于我的更多：
* <a markdown="0" href="https://github.com/peiweichen">Github</a>
* <a markdown="0" href="https://www.linkedin.com/in/peiwei233">Linkedin</a>
* <a markdown="0" href="https://www.instagram.com/peiweichen/">Instagram</a>
* Email    : peiwei233@gmail.com
* Phone   : +(86)13128981404
* Wechat : 13128981404





