---
layout: default
nav_exclude: true
---

# Microsoft Project Acoustics 声波物理模拟测试

***
<!-- Start Document Outline -->

* [What is Project Acoustics](#what-is-project-acoustics)
* [Workflow](#workflow)
	* [Plugin Integration](#plugin-integration)
	* [Bake Process](#bake-process)
		* [Objects](#objects)
		* [Materials](#materials)
		* [Probes](#probes)
		* [Bake](#bake)
	* [Design Control](#design-control)
		* [Dynamic Portal](#dynamic-portal)
* [Playtest Conclusion](#playtest-conclusion)

<!-- End Document Outline -->
***

[Project Acoustics](https://docs.microsoft.com/en-us/gaming/acoustics/what-is-acoustics) 是 Microsoft 的一个声学效果实现方案，用于在游戏等 3D 交互项目中实现 Occlusion、Obstruction 和 Reverb 等效果。该方案最早源自研究项目 [Project Triton](https://www.microsoft.com/en-us/research/project/project-triton/)，发展至今已有十多年，也已在一些游戏项目中经过验证。之前在[《展望游戏音频设计的发展方向》](What-will-The-Next-Gen-of-Game-Audio-Design-be-like.md)文中的声学环境建模（Acoustic Environment Modeling）章节中做过简单介绍，最近[该方案](https://www.unrealengine.com/marketplace/en-US/product/project-acoustics-for-unreal-audio)以 Engine Plugin 的形式发布至 UE 商城，正式支持 UE5 版本，整合流程和设计功能与之前相比也有了较大的改进，趁此时机正好对 Project Acoustics 做一个全面深入的测评。

## What is Project Acoustics

目前 3D 环境声学效果实现的各种方案大多都是建立在界定 Room 和 Portal 并进行 Raytracing 的思路之上，而 Project Acoustics 则采用了一种完成不同的方式，类似于光照烘焙（Static Light Baking），将复杂环境中声波传播的真实效果进行离线运算并存储下来，并从中提取重要参数用于设计控制和实时运算。

> Project Acoustics' philosophy is similar to static lighting: bake detailed physics offline to provide a physical baseline, and use a lightweight runtime with expressive design controls to meet your artistic goals for the acoustics of your virtual world.

这种方案避免了音频设计师依据美术场景手动界定 Room 和 Portal 的工作量，降低了 Raytracing 的实时运算消耗，另外特别是在一些复杂几何环境中的声学效果会更为精准可控。在获得了离线模拟数据的基础上，音频设计师可以更多关注在对音频参数的实时控制上来获得预想的效果。

> Project Acoustics' simulation captures these effects using a wave-based simulation. The acoustics are more predictable, accurate and seamless.  
> Project Acoustics' key innovation is to couple real sound wave based acoustic simulation with traditional sound design concepts. It translates simulation results into traditional audio DSP parameters for occlusion, portaling and reverb. The designer uses controls over this translation process.

需要说明的是，Project Acoustics 并不是取代了目前已有的工具或方案，而是可以看作整个音频空间化（Audio Spatialization）流程中对 3D 环境声学建模环节的另一种实现方式。Project Acoustics 本身并不直接参与定位和混响等声音效果的实现，而是对离线模拟数据中的信息进行处理，然后交给引擎或者音频中间件内的相应模块来实现最终效果，以更具适应性和扩展性的方式嵌入到已有的工作流中去。

## Workflow

目前官方提供了针对 Unreal、Unity 引擎和 Wwise 音频中间件的整合方案，并提供了相应的示例工程，大家可以前往项目官网下载测试。下文将以 Unreal Audio 整合方案和自制地图关卡为例，测试整体使用流程和最终效果。

### Plugin Integration


### Bake Process


#### Objects

#### Materials

#### Probes

#### Bake

### Design Control

#### Dynamic Portal


## Playtest Conclusion


最后，推荐观看



希辰  
2022.5.22

***