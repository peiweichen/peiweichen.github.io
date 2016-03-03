---
layout: post
title: "深入浅出Swift的Optional?!"
author: billy_rick
modified:
excerpt: "A post to test ?!"
tags: [iOS,Swift,tutorial,translation]
---

当苹果引进Swift的时候，他们有三个主要目标：安全，时尚和强大。其中包含这些特色的就是Swift的Optional.大部分编程语言特色的存在是为了能解决问题，而optional是为了解决问题而自然而然被创造。好了，开始举例。

假设你有一个张 没交房租住客名字的黑名单，黑名单里有公寓的号码.我们需要检查一个公寓号码是否在黑名单里面：

`let 黑名单 = ["101","202","303","404"]`

写一个非常简单的函数来检查一个公寓号码是否在一个名单里面：

func findApt (欠租人 : String, 黑名单: String[]) -> String {
for tempAptNumber in 黑名单 {
if ( tempAptNumber == 欠租人) {
return 欠租人
}
}
}
上面的函数findApt有两个参数：A String 欠租人 和 an array of strings 黑名单.

第一个参数是我们在寻觅的公寓号码，第二参数是一个公寓号码的数组（即黑名单）.这个函数一个一个搬出数组的公寓号码存储在临时变量`tempAptNumber`.然后我们比较公寓号码，如果确实有匹配，返回公寓号码.然而，上述的函数并不完整，因为我们只在找到匹配的公寓号码的时候才返回一个值.假如遍历这个数组名单结束了，还是没有找到匹配的公寓号码，这是函数要返回什么呢？

理想化地说，我们应该返回一个`nil`，但是我们已经设置了`string`作为我们的返回类型，一个解决问题的方法是返回一个empty string:

func findApt (欠租人 : String, 黑名单 : String[]) -> String {
for tempAptNumber in 黑名单 {
if ( tempAptNumber == 欠租人) {
return 欠租人
}
}
return ""
}

上述其实是一个失败的函数.因为我们不能使用下述的`if`语句：

if findApt("505", 黑名单) {
发送警告()
}

上述的`if`语句块永远都不能执行`sendNotice()`,因为`findApt`永远是`return aptNumber` or`return ""`,科普一下`""`不等同于`false` . `if`必须跟`bool表达式`.所以，上述函数可以改成：

if findApt("505", 黑名单) != "" {
发送警告()
}
但是这和我们的本意相违背了,代码不友好。
真是让人头疼，所过我们把`return ""` 改成 `return nil` ，我们编译不会通过因为返回类型是`string`.`这种情况就是optionals出场的时候了`

`An optional一个可变值` 有两种状态，一个值或者`nil(空)`.声明一个变量的时候，后缀加`？`即是可变值，例如`var optional?`.
我们把`optional`应用到我们的函数里面：

``` 
func findApt (aptNumber : String, 黑名单: String[]) -> String? {
for tempAptNumber in 黑名单 {
if ( tempAptNumber == 欠租人) {
return 欠租人
}
}
return nil
}
```
返回类型是一个`String?`所以我们在函数中可以返回nil.
现在我们的函数就拥有高端配置，轻松使用`if`语句：

```
let 欠租人 = findApt("404",黑名单)
if 欠租人{
发出警告();
}
```
现在我是向全公寓的住客`发出警告()`,假如我们需要向一个特定欠租的住客`发出警告(apt:Int)`.这种情况下，你可能会这种写我们的函数：

```
let 欠租人 = findApt("404",黑名单)
if 欠租人{
发出警告(欠租人);
}
```
这样子，编译器会报错.因为欠租人这个常量是一个`optional可变值`,因为它是我们函数的返回类型.上述的语句等同下面的语句：

```
let 欠租人: String? = findApt("404",aptNumbers)
if 欠租人{
发出警告(欠租人)
}
```
如果我们将`发出警告(欠租人)`改成`发出警告(欠租人！)`，即：

```
let 欠租人: String? = findApt("404",aptNumbers)
if 欠租人{
发出警告(欠租人!)
}
```
欠租人!实际上等同一个String类型，而欠租人是String?类型。
加了`?`号实际上是把`欠租人`这个常量包裹(wrap)起来，把nil和它包在一起;`!`把`欠租人`这个常量解开(unwrap)，让它变回原来的类型。所以在
`发出警告(欠租人!)`不会出现 `发出警告(nil)`的情况.

欠租人是一个String类型，我们把它转换成Int:

```
let 欠租人 = findApt("404",aptNumbers)
if 欠租人{
发出警告(欠租人!.toInt())
}
```
上述`if`语句其实不能执行的，因为`if`只能判断`true`或`false`,而不能判断`String`或`nil`.我们应该改成:

```
if let 欠租人 = findApt("404",aptNumbers){
发出警告(欠租人.toInt())
}
```
可能你发现`欠租人!.toInt()`的感叹号去掉了,这...?

因为当使用`if let`语句时,常量`欠租人`会被赋予真正的值，即`欠租人=非空的String或空String`,二选一，不是一个可变.

还有一个问题没有解决，`toInt()`返回`Int?`类型。
所以，我们可以这样:

```
if let 欠租人 = findApt("404",aptNumbers){
发出警告(欠租人.toInt()!)
}
```
但是这样可能会出现`发出警告(nil)`的情况出现，为了应对这种情况，我们可以加一个`if let`语句判断，当 `欠租人.toInt()`为`nil`不`发出警告()`

```
if let 欠租人 = findApt("404",aptNumbers){
if let 欠租人Int = 欠租人.toInt() {
发出警告(欠租人Int)
}
```

Swift还提供一种`optional chaining可变连接`，精简上述的语句如下：

```
if let 欠租人Int = findApt("404",aptNumbers)?.toInt()
{
发出警告(欠租人Int)
}
```

结论:
`Optionals可变参数`可以帮助我们节省大量时间和让我们的代码更具可读性和效率.我们可能需要一段时间来习惯它，但是一旦你掌握了它你会讨厌看到写代码时要去不断精密地检查`nil`或者`sentinel values`

本文是本人(www.ipeiwei.xyz)敲键盘的翻译文章，原文出处：[原文]("http://blog.teamtreehouse.com/understanding-optionals-swift") for English.




