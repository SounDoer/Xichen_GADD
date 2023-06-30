---
layout: default
nav_exclude: true
---

# Object-Based Audio 音频设计与混音案例

***
<!-- Start Document Outline -->

* [Sub Title A](#sub-title-a)
	* [Sub Title B](#sub-title-b)

<!-- End Document Outline -->
***

作为一款支持 PC 和主机平台的第三人称射击游戏，[《SYNCED》](https://www.syncedthegame.com/)在项目早期就确定了高规格的音频交付标准，在保证绝大多数玩家使用耳机的听感效果的同时，还需要适配立体声、多声道环绕声以及带有顶置音箱的影院级配置的各种听音环境。音频中间件 Wwise 在 2021 版本中引入了 Object-Based Audio Pipeline（基于对象的音频管线），配合各个厂商如 Dolby 和 Sony 等在终端设备上提供了更多的渲染支持，《SYNCED》音频组也在第一时间跟进，对项目音频结构和工作管线进行了改造，利用新的工具在各类终端和回放环境下实现高规格的音频交付标准。趁此游戏即将上线之际，本文将以《SYNCED》项目为例，从 Object-Based Audio 这个核心概念出发，分享一下在资源制作、声音定位、管线结构和混音策略等方面的工作细节与经验得失。

## What is Object-Based Audio and Why?



概念澄清
Spatial Audio
Object Based Audio
Dolby Atmos

目的
Endpoint OBA Realization
Multiple devices supported with less workload
Performance Optimization

整体流程
素材制作
……
混音棚混音

开发流程
Wwise structure details

静态混音
动态混音：Mixing Preset / Game Parameter Control

Mastering: according to devices, scene and style

## Sub Title A

[Link](https://en.wikipedia.org/wiki/Cutscene)

### Sub Title B

![PicLink](Audio-Design-Pipeline-of-Realtime-Cinematic-in-Object-Based-Audio/RealtimeCinematic.png)


希辰  
2022.3.29

***