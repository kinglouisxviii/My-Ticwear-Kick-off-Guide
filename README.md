## 我的ticwear应用开发指南
#### 前言
最近着手开发一个基于ticwear智能手表操作系统的应用，从完全摸不到头脑到把调通ticwear的API，中间经历了不少迷惑。所以我决定制作一个开发调试的图文详解，减少大家在编写安卓程序之外浪费的时间。虽然这里官方的[快速入门](http://developer.chumenwenwen.com/v2/doc/ticwear/getting-started)有一个大概的脉络，但是我还是希望写一个比较个人化的一个实践指南。 

#### 背景知识
Ticwear是什么？我的理解就是它是一个由于谷歌服务在中国无法落地的产物。它是一个中文智能手表操作系统，何以运行平时我们写的Android手表程序。**同时，他还有一套比较完整的通讯API去代替Google Play Service.** 装载Ticwear操作系统的智能手表通过Ticwear助手这个软件与安卓或者苹果手机相连。所以从某种意义上来讲，Ticwear助手讲代替Android Wear（这里指运行在安卓手机上的一个助手软件）的工作。

#### 图文调试教程
##### 连接手表与Ticwear助手
这里我们演示使用的是小米红米手机，和moto360一代（已刷成Ticwear系统）
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/8B1A9233-910F-4407-87E5-3AF18AEF35A9.jpg)

#### 用Android Studio编写一个简单的程序
这里我们用Android Studio编写一个最最基础的Hello World程序。这里截图用的是Android Studio 2.1.1。

##### 创建一个mobile + wear的安卓项目
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/as_1.jpg)
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/as_2.jpg)

创建完成后Android Studio显示的界面如下图
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/as_3.png)

这里用的是Android Studio的默认设置，所以在mobile和wear的build.gradle中分别看到
```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    wearApp project(':wear')
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.google.android.gms:play-services:9.0.0'
}
```
和
```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.android.support:wearable:2.0.0-alpha1'
    compile 'com.google.android.gms:play-services-wearable:9.0.0'
}
```
其中com.google.android.gms:play-services:9.0.0和compile 'com.google.android.gms:play-services-wearable:9.0.0'就代表了把Google Play Service编译到程序里。然而前面说到这个东西在中国目前是无法正常使用的，这也是出门问问公司做的这套SDK的用武之地了！

[SDK传送门](https://github.com/ticwear/sdk/raw/master/lib/mobvoi-api.jar)
[API使用说明](http://developer.chumenwenwen.com/v2/doc/ticwear/wearable-api)

把mobvoi-api.jar下载并移动到mobile和wear目录下的libs文件夹内（两份），在上面build.gradle的dependencies里将compile 'com.google.android.gms:play-services:9.0.0'和compile 'com.google.android.gms:play-services-wearable:9.0.0'分别注释掉后，添加compile files('libs/mobvoi-api.jar')即可调用。使用API的图文演示会放在后面。

#### 将程序导入手机和手表中
由于moto360无法通过USB线与电脑相连，这里演示两种调试方法。蓝牙调试和wifi调试。
##### 蓝牙调试
##### wifi调试
