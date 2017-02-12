---
title: PlistBuddy 简单使用
tags: iOS
---
plist 是 OSX 中非常普遍的一种文件格式，类似 XML，通过键值对的方式来进行一些配置。而 PlistBuddy 则是 OSX 系统自带的专门解析 plist 的小工具（Buddy 为好朋友，伙伴的意思。从名字不难看出 PlistBuddy 对 plist 文件的友好支持）。

由于 PlistBuddy 并不在 OSX 默认的 Path 里，所以我们得通过绝对路径来引用这个工具：

例如查看帮助，我们需要在终端上输入以下命令：
```ruby
/usr/libexec/PlistBuddy --help
```
<!--more-->
下面我们来看看 PlistBuddy 的简单使用
### 打印：

初始化一个 info.plist 文件：

![初始化info.plist](http://upload-images.jianshu.io/upload_images/1760370-6c0ea39c931bb149.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打印 info.plist 文件：

```ruby
/usr/libexec/PlistBuddy -c "print" info.plist
```
在终端输入上述命令后如下所示：
![](http://upload-images.jianshu.io/upload_images/1760370-e80e90fa064435cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 添加
添加普通字段:
```ruby
/usr/libexec/PlistBuddy -c 'Add :Version string 1.0' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-44d77fb02b78bb82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加数组字段，分两步走，注意 `key之间用 : 隔开，且不能有空格` ：
```ruby
# 先添加key值
/usr/libexec/PlistBuddy -c 'Add :Application array' info.plist
# 添加value值
yans67deMacBook-Pro:needfiles huangyg$ /usr/libexec/PlistBuddy -c 'Add :Application: string app1' info.plist
yans67deMacBook-Pro:needfiles huangyg$ /usr/libexec/PlistBuddy -c 'Add :Application: string app2' info.plist
yans67deMacBook-Pro:needfiles huangyg$ /usr/libexec/PlistBuddy -c 'Add :Application: string app3' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-59c60d6f57b54a3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加字典字段，分两步走：
```ruby
# 先添加key值
/usr/libexec/PlistBuddy -c 'Add :Person dict' info.plist
# 添加value值,
/usr/libexec/PlistBuddy -c 'Add :Age string secret' info.plist
/usr/libexec/PlistBuddy -c 'Add :Person:Name string yans67' info.plist
/usr/libexec/PlistBuddy -c 'Add :Person:sex string boy' info.plist
/usr/libexec/PlistBuddy -c 'Add :Person:weight string 65' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-329d4e8641b66aca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 输出
打印字段相应的值：

```ruby
 /usr/libexec/PlistBuddy -c 'Print :Person' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-16e7819baebffaa9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
在array中我们还可以根据下标打印某个特定的值

```ruby
/usr/libexec/PlistBuddy -c 'Print :Application:2' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-817251d0a736cb0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 删除
删除字段相应的值：

```ruby
/usr/libexec/PlistBuddy -c 'Delete :Version' info.plist
```

![](http://upload-images.jianshu.io/upload_images/1760370-e5c4b45f13f004ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 修改
修改某个字段相应的值：

```ruby
/usr/libexec/PlistBuddy -c 'Set :Application:1 string "thi is app1"' info.plist

```

![](http://upload-images.jianshu.io/upload_images/1760370-305fa65a0a8875bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 合并
当有两个plist文件的时候，我们可以对其进行合并操作

```ruby
# 将A.plist 合并到 B.plist中
/usr/libexec/PlistBuddy -c 'Merge A.plist'  B.plist
```

![终端中会提示B.plist中有重复的键值，所以默认跳过该键值的合并
](http://upload-images.jianshu.io/upload_images/1760370-cff3a72a34d7b56d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![合并前](http://upload-images.jianshu.io/upload_images/1760370-9421a55a5721c22d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![合并后](http://upload-images.jianshu.io/upload_images/1760370-1ce3fb03a059fb39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)