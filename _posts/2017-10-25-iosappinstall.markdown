---
layout:     post
title:      "IOS模拟器安装APP"
subtitle:   "在 iPhone X 模拟器中安装APP，适配必备"
date:       2017-10-25 12:00:00
author:     "Lilya"
header-img: "img/postPic/jekyll-bg.jpg"
catalog: true
tags:
    - IOS
    - iPhone X
---

## 一、mac系统升级
如果需要 iPhone X 模拟器，需要先升级系统至最新版本，当前版本是Version 10.13 (17A405)
## 二、xcode安装
直接在 App Store 中下载安装，当前版本是Version 9.0.1 (9A1004) 
## 三、通过命令行安装APP
1. **启动模拟器**
```
open -n /Applications/Xcode.app/Contents/Developer/Applications/Simulator.app --args -currentDeviceUDID 80788976-098C-4AAF-8427-2B8EBF5A2350
```
>Tips: currentDeviceUDID 可以通过命令``xcrun simctl list``获取
2. **安装APP**
```
xcrun simctl install booted /path/xxx.app
```
3. **唤起APP**
```
xcrun simctl launch [-w | --wait-for-debugger] [--console] [--stdout=<path>] [--stderr=<path>] <device> <app identifier> [<argv 1> <argv 2> ... <argv n>]xcrun simctl launch <device> <app identifier>
```
常用的是命令：``xcrun simctl launch booted com.xxx.xxx``
>tips:在Finder中找到xxx.app中，点击``显示包内容``，找到``Info.plist``文件，双击进入，可查看基本信息，比如版本号、Bundle identifier


## 四、问题及解决方案
1. xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
**解决方案**：```xcode-select --install```，改命令会安装 xcode developer tools ，完成即可。

2. xcrun: error: unable to find utility “simctl”, not a developer tool or in PATH`
**解决方案**：``sudo xcode-select -switch Xcode路径/Contents/Developer``，通常Xcode路径为``/Applications/Xcode.app``,可通过将应用程序中的 Xcode 拖进 terminal 获取其路径。
