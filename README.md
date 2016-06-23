# 我的ticwear应用开发指南
### 前言
最近着手开发一个基于ticwear智能手表操作系统的应用，从完全摸不到头脑到把调通ticwear的API，中间经历了不少迷惑。所以我决定制作一个开发调试的图文详解，减少大家在编写安卓程序之外浪费的时间。虽然这里官方的[快速入门](http://developer.chumenwenwen.com/v2/doc/ticwear/getting-started)有一个大概的脉络，但是我还是希望写一个比较个人化的一个实践指南。演示主要在调试和打包上。

### 背景知识
Ticwear是什么？我的理解就是它是一个由于谷歌服务在中国无法落地的产物。它是一个中文智能手表操作系统，何以运行平时我们写的Android手表程序。**同时，他还有一套比较完整的通讯API去代替Google Play Service.** 装载Ticwear操作系统的智能手表通过Ticwear助手这个软件与安卓或者苹果手机相连。所以从某种意义上来讲，Ticwear助手讲代替Android Wear（这里指运行在安卓手机上的一个助手软件）的工作。

### 图文调试教程
##### 连接手表与Ticwear助手
这里我们演示使用的是小米红米手机，和moto360一代（已刷成Ticwear系统）
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/8B1A9233-910F-4407-87E5-3AF18AEF35A9.jpg)

### 用Android Studio编写一个简单的程序
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

把mobvoi-api.jar下载并移动到mobile和wear目录下的libs文件夹内（两份），在上面build.gradle的dependencies里将compile 'com.google.android.gms:play-services:9.0.0'和compile 'com.google.android.gms:play-services-wearable:9.0.0'分别注释掉后，添加compile files('libs/mobvoi-api.jar')即可调用。使用API请参考官方的[demo](https://github.com/ticwear/sdk/tree/master/sample/android-studio)。

### 将程序导入手机和手表中
最最简单的程序已经写好了，由于moto360无法通过USB线与电脑相连，这里演示蓝牙调试。
##### 蓝牙调试
在安卓手机上打开ticwear助手，点击右上角的按钮后点击“更多”。进入新界面后**连续点击版本号数次启动开发者模式**。
然后进入手表的设置->点击开发者选项->点击通过蓝牙调试, 同时开启ticwear的开发者蓝牙调试，现在屏幕上应该显示为：
```
Host: disconnected
Target: connected
```
这里Host指的是你的电脑，Target指的是你的智能手表。
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/9FF18EA7-E337-4EE3-9272-D1E0F942FB96.jpg)
**常见问题**：
我经常遇到手表和手机已经相连但是仍显示
```
Host: disconnected
Target: disconnected
```
**解决办法**：
用ticwear助手切换表盘确认手表与手机确实相连，之后关闭ticwear助手里的开发者蓝牙调试的开关（上图第四格图片所示），然后再打开开关。

现在打开你手机中的开发者选项，启动USB调试功能。具体各个安卓手机情况不太一样，这里请各位看官自行调查=）
启动USB调试功能后，用USB连接线连接手机和电脑。
执行如下指令查看连接在电脑上的设备（adb怎么安装大家自行解决）
```
adb devices
```
将会看到现在连接的设备
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/connect1.png)
然后我们将我们的电脑作为一个hub，执行如下命令
```
adb forward tcp:4444 localabstract:/adb-hub
```
```
adb connect localhost:4444
```
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/connect2.png)
**常见问题**：
这里如果执行adb connect localhost:4444失败，可以通过执行
```
adb kill-server
```
然后重新执行 
```
adb forward tcp:4444 localabstract:/adb-hub 
```
```
adb connect localhost:4444
```
来解决。
现在我们再打开ticwear助手，就可以看到
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/IMG_6018.jpg)
然后在电脑的terminal里再次执行
```
adb devices
```
会看到蓝牙设备已经连上。
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/connect3.png)

现在在Android Studio中选择wear然后点击play将会出现moto360这个设备：
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/connect4.png)

点击OK，我们的程序就成功的拷贝到手表上啦！

### 打包演示
打包会将我们的应用生成为.apk，你可以通过adb install将apk拷入手表或者手机，也可以把apk文件提交到出门问问的商店里。
![alt tag](https://github.com/kinglouisxviii/My-Ticwear-Kick-off-Guide/blob/master/tutorial_pics/publish.jpg)
在Android Studio下点击"Build"下的"Generate Signed APK...", 然后选择mobile，创建或者选择已有的key，选择release模式，选择生成地址产生你的APK。现在你会发现在桌面上有两个.apk文件：mobile-release.apk和wear-release.apk。你可以将手表的apk直接通过
```
adb -s localhost:4444 install ~/Desktop/wear-release.apk
```
的方法装入手机，也可以通过
```
adb -s 6O5614A19926 install ~/Desktop/mobile-release.apk
```
的指令把包装入手表（当然6O5614A19926你应该替换成你手表device的代码），再在ticwear助手点击右上角的按钮，点击“应用同步”。因为mobile-release.apk相当于是手机应用与手表应用的打包，所以手表的应用也会被拷贝到手表上！
详情参考[这里](http://developer.chumenwenwen.com/v2/doc/ticwear/gms-compat)的“打包、兼容、以及各类概念的解释”。

###### 2016 Ren Lu © All Rights Reserved.
######License: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode)

