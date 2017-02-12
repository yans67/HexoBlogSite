---
title: 关于SizeClasses
tags: iOS
---
#### <a id="2-SizeClasses简介"></a>SizeClasses简介

`Size Classes` 为 iOS 8 的新特性，其特点就是将各个设备屏幕及其旋转后状态都抽象成屏幕 Size 的变化，而不再关心具体的尺寸。具体就是将屏幕的宽度抽象成Compact（紧凑型）、Any（任意）、Regular（常规）三种情况，高度也是如此，因此就有9种情况。简单的说，就是在一个 Storyboard 或者 xib 上，能够管理9种类型的视图。

#### <a id="3-SizeClasses与AutoLayout"></a>SizeClasses与AutoLayout
1. Size Classes 只是将视图分类，AutoLayout 才是对视图进行布局
2. Size Classes 依赖于 AutoLayout。关闭 AutoLayout，则 Size Classes 无法开启。关闭Size Classes，对 AutoLayout 无影响。
<!--more-->
#### <a id="4-SizeClasses与设备size"></a>SizeClasses与设备size
![](http://upload-images.jianshu.io/upload_images/1760370-cab416cbff8e5ed6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/1760370-46368711ec69918e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果没有特殊指定Size的类型，则默认是在 `wAny hAny` 模式下设置。其它8种类型继承于这个类型。

1. 在 storyboard 中添加一个 imageView，将imageView的图片设置如下。对图片设置好约束后，可以在 Xcode 右侧的属性选项中看到 `installed` ，默认表示为 `AA`。我们可以看到下面 Preview 内容在所有设备类型中都是显示这张图片。
![](http://upload-images.jianshu.io/upload_images/1760370-6a3e2f2c2d57a3d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 现在我们来对 imageView 进行操作，点击 `installed` 左边的 `＋`，添加 `C R` 类型的视图 (`C R`)。
![](http://upload-images.jianshu.io/upload_images/1760370-0837e3b01e0b1906.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 取消 `C R` 的打勾项☑️，此时我们可以从 Preview 中看到Size类型为`C R` 的都不显示 imageView。
![](http://upload-images.jianshu.io/upload_images/1760370-25d261d031e86d3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 只有选择 installed 之后，图片才会被绘制在View上，如果没有选择 installed，则不会绘制在 View 上，左图为选择 installed，右图没有选择installed：
![](http://upload-images.jianshu.io/upload_images/1760370-ca23f21852f4b1b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1760370-994471e14cc4a169.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 简单的三个步骤，我们就能实现 imageView 在 ipad 竖屏上显示，在iphone竖屏不显示。
> 同理，如果你想让 imageView 只在 iphone 竖屏上，则将默认的installed（第一个）取消打勾，然后对 `C R`的 installed 打勾。

再举个简单的例子：
1. 现在我们想让 imageView 只在 iphone 6Plus（5.5寸）横屏上不显示图片。我们可以如下操作：
![](http://upload-images.jianshu.io/upload_images/1760370-aa46ad92d4b1cad6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 然后在右边中添加 `RC` 的 installed，取消打勾，此时效果如下，只有 6Plus 无法显现 imageView。
![](http://upload-images.jianshu.io/upload_images/1760370-a6d2f80b2fcb9675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### <a id="5-SizeClasses与assets"></a>SizeClasses与assets

不仅视图可以用 Size Classes 来进行抽象分类，Image Asset 也能支持 Size Classes，用法类似。如下图：
![](http://upload-images.jianshu.io/upload_images/1760370-1d500f7eab379a02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过选择 Width 和 Height 的类型可以设置不同类型对应不同的图片 `(*表示A、+表示R、－表示C)`，例如：
![](http://upload-images.jianshu.io/upload_images/1760370-25108b9dc1b26d8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不过设置 assets 的时候不能通过 Preview 来实时观察图片的变化，所以只能运行模拟器，可以发现选择的时候会默认改变图片：
![](http://upload-images.jianshu.io/upload_images/1760370-a166c17d8a61d3e2.gif?imageMogr2/auto-orient/strip)


我们可以打开 image 的 content.json，查看里面的具体内容：
![](http://upload-images.jianshu.io/upload_images/1760370-93d0fbbbac82a903.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以发现，image 的 content.json 数据中，1x的图片多出了`width-class` 和 `height-class` 两个键值。
![](http://upload-images.jianshu.io/upload_images/1760370-86aad31d6ce9c1c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 可以看到，旋转时候如果想要改变图片，我们再也不用

#### <a id="6-SizeClasses与Constraints"></a>SizeClasses与Constraints
前面提到，Size Classes 是依赖于 AutoLayout，所以设置 Constraint 的时候也可以设置 Size Classes 的类型，例子如下：

1. 回到 storyboard，点击其中的一条约束，如下图：
![](http://upload-images.jianshu.io/upload_images/1760370-d9b09173c7daf810.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 添加 `R C` 的约束：
![](http://upload-images.jianshu.io/upload_images/1760370-e5def5b9e9e809d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 在 Preview 中可以看到，6Plus 横屏下 image 的高度变成200。

#### <a id="7-SizeClasses适用场景"></a>SizeClasses适用场景

1. 同个工程，同一个视图，不同设备上（iPhone，iPad）显示或隐藏；
2. 多种屏幕类型中使用同一个imageView，但图片不同；
3. 多种屏幕类型中使用同一个视图，但视图的约束不一致。


#### <a id="8-SizeClasses代码使用"></a>SizeClasses代码使用
为了 Size Classes，Apple 在 iOS 8 中引入了一个新的类，UITraitCollection。这个类封装了像水平和竖直方向的 Size Class 等信息。

iOS 8 的 UIKit 中大多数 UI 的基础类 (包括 UIScreen，UIWindow，UIViewController 和 UIView) 都实现了 UITraitEnvironment 这个接口，通过其中的 traitCollection 这个属性，我们可以拿到对应的 UITraitCollection 对象，从而得知当前的 Size Class，并进一步确定界面的布局。