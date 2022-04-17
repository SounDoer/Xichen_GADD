---
layout: default
nav_exclude: true
---

# 基于对象音频的游戏实时动画音频设计流程

本文将以游戏《SYNCED: Off-Planet》（重生边缘）中的开场剧情关卡为例，展开详述 UE 引擎结合 Wwise 音频中间件的开发环境下，基于对象音频（Object Based Audio）的游戏实时动画（Realtime Cinematic）音频设计流程。

## Realtime Cinematic

首先，本文讨论的实时动画是指穿插在游戏进行过程中非玩家控制的线性叙事部分 [Cutscene](https://en.wikipedia.org/wiki/Cutscene)（过场动画），称为 Realtime Cinematic 是为了强调其两个特点：一是 Realtime，区别于播放预渲染流媒体视频的实时方式，与游戏 Gameplay 部分之间有更强的联系，实现过场动画与玩家镜头之间的无缝衔接；二是 Cinematic，无论是预渲染还是实时渲染，从最终的呈现效果上来看都是戏剧性的线性影视化内容，也就是说实时动画的音频设计目标和标准与制作线性内容是一致的。

《SYNCED: Off-Planet》

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

### 改进

希辰  
2022.3.29