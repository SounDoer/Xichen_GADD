---
layout: default
nav_exclude: true
---

# 基于对象音频的游戏实时动画音频设计流程

本文将以游戏《SYNCED: Off-Planet》（重生边缘）中的开场剧情关卡为例，展开详述 UE 引擎结合 Wwise 音频中间件的开发环境下，基于对象音频（Object Based Audio）的游戏实时动画（Realtime Cinematic）音频设计流程。

## Realtime Cinematic

首先，本文讨论的实时动画是指穿插在游戏进行过程中非玩家控制的线性叙事部分 [Cutscene](https://en.wikipedia.org/wiki/Cutscene)（过场动画），称为 Realtime Cinematic 是为了强调其两个特点：一是 Realtime，区别于播放预渲染流媒体视频的实时方式，与游戏 Gameplay 部分之间有更强的联系，比如过场动画与玩家镜头之间的无缝衔接和声音内容的平滑过渡，提供更为连续的体验感；二是 Cinematic，虽然具体的实现方式和工具流程有所区别，但无论是预渲染还是实时渲染，从最终的呈现效果上来看都是线性的影视化内容，因此实时动画音频设计的听感目标和评价标准与制作线性内容也是一致的，需要体现出线性内容声音设计在细节处理和整体混音上的特点，同时在技术规格上也要符合要求，比如对耳机和音箱等各类不同声道重放设备的适配。


如上图，《SYNCED: Off-Planet》开场约二十分钟的流程中有大小近十个过场动画，串联起了整个关卡各个阶段的剧情发展和情绪推动。在 UE 引擎中，每个过场动画都是一个独立的 Level Sequence，其中包含了当前场景中的各个角色、物件和摄像机等对象，在各个对象基于时间轴的轨道上添加关键帧等内容来完成场景调度。多处过场动画的开头和结尾都采用了 Camera Blend，实现动画镜头至玩家镜头的无缝衔接。



### The Goal of Cinematic Sound

### The Specifications of Cinematic Sound

## Object Based Audio

### Channel Based vs. Object Based

意义在于

### Wwise Object-Based Audio

https://blog.audiokinetic.com/working-with-object-based-audio/
https://blog.audiokinetic.com/authoring-for-audio-objects-in-wwise/
https://www.audiokinetic.com/library/edge/?source=Help&id=object_based_audio_overview

https://games.dolby.com/atmos/wwise/

## Audio Design Pipeline

### 数据对比

开发中的优势，针对频繁改动

前后数据量对比

### 改进

希辰  
2022.3.29