---
layout: post
title: "图解UIScrollView的contentOffset和contentInset"
author: peiwei
modified:
excerpt: "feeling the contentOffset and contentInset properties of the UIScrollView class"
tags: [iOS,Swift,tutorial,scrollview,contentinset,contentoffset]
---

第一步，理解scrollview的content view，例如我们平时在刷微博，推特时，scrollview的content view会比屏幕大。
看一下下图,content view用 '#'围起来，用‘－’围起来的方形是我们的手机屏幕：

``` 
   ############
   #          #
 __#__________#__
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
 --#----------#--
   #          #
   ############
```

#### contentOffset
上图中，很明显content view大于我们的手机屏幕

 
关于contentOffset,我们看例子：

``` 

contentOffset = CGPoint(x:0.0, y:0.0)

________________ <------ content view's y = 0
|  ############  |
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
 --# ---------#--  
   #          #  
   #          #  
   #          #
   #          #
   ############
```
   


```
contentOffset = CGPoint(x: 0.0, y: 40.0)  

   ############
   #    40    #
 __#__________#__  <------ content view's y = 40
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
|  #          #  |
 --#----------#--
   #          #
   ############  
```



```
contentOffset = CGPoint(x: 0.0, y: -80.0)  
 ________________  <------ content view's y = -80
|                |
|       80       |
|                |
|  ############  |  
|  #          #  |
|  #          #  |
 --# ---------#--  
   #          #  
   #          #  
   #          #
   #          #
   #          #
   #          #
   ############ 
   
   所以当你把scrollView往下拉到顶部，继续往下拉的时候，contentOffset就是负数。
```

如果看了这三个例子你不能理解contentOffset是什么，好吧，再看一遍。

####    contentInset
 contentInset可以为scrollview内嵌更多（更少）可滑动的空间，contentInset.top = 10会在scrollView的顶部增加10的多余可滑动空间，scrollview初始的顶部有10的空白,而且这个空白会跟随scrollview滑动。
 看例子：

``` 
//UIEdgeInsetsMake(top,left,bottom,right)
 contentInset = UIEdgeInsetsMake(66.0, 0.0, 0.0, 0.0)
scrollview初始的时候，顶部有66的非内容区域，scrollview的内容只在内容区域展示
 ________________  
|                |
|       66       |
|  ############  | 
|  #          #  |
|  #          #  |
 --# ---------#--  
   #          #  
   #          #  
   #          #
   #          #
   #          #
   #          #
   ############ 
```


```
contentInset = UIEdgeInsetsMake(0.0, 0.0, 66.0, 0.0)
// scrollview初始的时候，底部有66的非内容区域，scrollview的内容只在内容区域展示
   ############   
   #          #  
   #          #  
   #          # 
   #          #  
   #          #  
 __#__________#__
|  #          #  |
|  #          #  |
|  #          #  |
|  ############  |
|                |
|      	66       |
 ----------------

```





