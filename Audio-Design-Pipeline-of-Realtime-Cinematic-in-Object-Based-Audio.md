---
layout: default
nav_exclude: true
---

# 基于对象音频的游戏实时动画音频设计流程

***

<!-- Start Document Outline -->

* [Realtime Cinematic](#realtime-cinematic)
	* [The Specifications of Cinematic Sound](#the-specifications-of-cinematic-sound)
* [Object Based Audio](#object-based-audio)
	* [Channel Based vs. Object Based](#channel-based-vs-object-based)
	* [Wwise Object-Based Audio](#wwise-object-based-audio)
* [Audio Design Pipeline](#audio-design-pipeline)
	* [数据对比](#数据对比)
	* [改进](#改进)

<!-- End Document Outline -->

***

本文将以游戏《SYNCED: Off-Planet》（重生边缘）中的开场剧情关卡为例，展开详述 UE 引擎结合 Wwise 音频中间件的开发环境下，基于对象音频（Object Based Audio）的游戏实时动画（Realtime Cinematic）音频设计流程。

## Realtime Cinematic

首先，本文讨论的实时动画是指穿插在游戏进行过程中非玩家控制的线性叙事部分 [Cutscene](https://en.wikipedia.org/wiki/Cutscene)（过场动画），称为 Realtime Cinematic 是为了强调其两个特点：一是 Realtime，区别于播放预渲染流媒体视频的实时方式，与游戏 Gameplay 部分之间有更强的联系，比如 Cutscene 与玩家镜头之间的无缝衔接和声音内容的平滑过渡，提供更为连续的体验感；二是 Cinematic，虽然具体的实现方式和工具流程有所区别，但无论是预渲染还是实时渲染，从最终的呈现效果上来看都是线性的影视化内容，因此实时动画音频设计的听感目标和评价标准与制作线性内容也是一致的，需要体现出线性内容声音设计在细节处理和整体混音上的特点，同时在技术规格上也要符合要求，比如对耳机和音箱等各类不同声道重放设备的适配。


如上图，《SYNCED: Off-Planet》开场约二十分钟的流程中有大小近十个 Cutscene，串联起了整个关卡各个阶段的剧情发展和情绪推动。在 UE 引擎中，每个 Cutscene 都是一个独立的 Level Sequence，其中包含了当前场景中的各个角色、物件和摄像机等对象，在各个对象基于时间轴的轨道上添加关键帧等内容来完成场景调度。多处 Cutscene 的开头和结尾都采用了 Camera Blend，实现动画镜头至玩家镜头的无缝衔接。

### The Specifications of Cinematic Sound

既然 Cutscene 是固定时长的线性序列，那是否可以在数字音频工作站中依据序列视频，直接制作出一条包含语音、音乐和音效且经过声音剪辑和混音的完整音频资源？虽然这种常规的 Channel Based（基于声道）线性内容声音制作方式能够最大程度地保证最终的整体效果，但是针对游戏实时动画的音频设计还是存在无法避免的局限，具体从以下几个方面来分析：

1. 语音是游戏中一个相对复杂的子系统，除了声音资源之外，还有字幕显示和多语言版本等问题需要共同考虑，因此 Cutscene 的语音需要统一在游戏整体的语音触发逻辑框架下，不能与其他声音内容直接混合。
1. 作为同一个叙事整体，Cutscene 和 Gameplay 部分之间需要顺畅的音乐过渡来转换情绪和推动剧情，使用 Wwise 中间件的互动音乐功能模块，设置 State 和 Transition 规则，可以方便地实现这一需求。因此 Cutscene 的音乐需要统一在整个关卡的音乐设计框架下，不能与其他声音内容直接混合。
1. Channel Based 音频资源的声道数目已经确定。如果以立体声格式来制作，那么在多声道重放环境下就会有明显的声道内容缺失；如果以最多声道数目的格式来制作，那么在较少声道的重放环境下就会出现自动下混的情况，实际响度和定位精度会出现偏差。而且更为严重的问题是，多声道格式音频资源中的低频、环绕和顶置声道通常会存在大量的空白片段，这些没有实际内容但是仍然占用了存储资源的数据，在内存资源分毫必争的游戏开发环境下来说，是一种极大的浪费。因此，Cutscene 的声音内容需要一种更为高效的声像摆位方式来解决不同声道重放设备的适配问题，不适合直接使用整段 Channel Based 的音频资源。

除了上述三点从声音内容角度分析的局限之外，许多厂商和平台也都早在所谓 Object Based（基于对象）方向上提供了各自的解决方案，比如 Microsoft Spatial Sound 和 Dolby Atmos 等，允许带有更多信息的元数据在终端播放环节进行计算来实现更高精度和自由度的声像定位效果，这些技术标准的普及也对音频设计流程提出了新的要求。

## Object Based Audio

Object Based Audio 并不是一个有着严格定义的行业术语，在不同语境和讨论范围下都有着各自不同的含义。下面仅从与本文案例有关的两个角度来做解释说明，一是游戏音频设计中的 Object Based 基本逻辑，二是 Wwise 中间件 2021.1 版本引入的 Object Based Audio Pipeline。

### Channel Based vs. Object Based

声音重放技术从单声道、双声道立体声、多声道环绕声、三维环绕声等一路发展至今，是以不断增加声道数量的方式来营造更全面的沉浸感的。这种 Channel Based 的重放技术也决定了以电影为代表的线性媒体声音制作方式是直接围绕声道展开的，根据画面内容在数字音频工作站中的声轨上通过声像定位和音量调节等方式在时间轴上精细地控制每一时刻从各个声道中输出的声音内容，来模拟画面内容中各个元素与观众之间的方位与距离关系。  
游戏音频的终端输出也需要考虑基于各种声道制式的重放设备，比如耳机、手机、电视机和家庭影院等。然而由于非线性和交互性的特点，导致游戏无法像电影那样通过输出多个确定版本的音轨来保证在各种设备上的重放效果，时间和空间上的不确定性决定了游戏中的声音内容输出需要以实时计算的方式来判断，因此在开发阶段的设计思路也与线性媒体声音制作有所不同，主要是 Object Based，而非 Channel Based。游戏音频设计中的 Listener（听者）和 Emitter（发声体）对象概念就是这种思路的典型体现，根据两者之间的相对关系来计算分配到终端重放设备各个声道的内容。  
上文提到许多厂商都已有了 Object Based 相关的解决方案，以 Dolby Atmos 为代表的在 Channel Based 线性内容制作流程中引入 Audio Object 的思路，其实就是在向游戏天然的音频设计逻辑看齐，尽可能摆脱声道因素对声音内容的影响。所以对于游戏中的实时动画而言，可以充分发挥游戏音频设计的先天优势，以 Objcet Based 的方式来完成这些线性内容的音频设计。



意义在于

### Wwise Object-Based Audio Pipeline

https://blog.audiokinetic.com/working-with-object-based-audio/
https://blog.audiokinetic.com/authoring-for-audio-objects-in-wwise/
https://www.audiokinetic.com/library/edge/?source=Help&id=object_based_audio_overview

https://games.dolby.com/atmos/wwise/


需要补充说明，目前的实际效果

## Audio Design Pipeline

### 数据对比

开发中的优势，针对频繁改动

前后数据量对比

### 改进

## 未来

### 基于对象音频的线性媒体音频制作工具

希辰  
2022.3.29

***