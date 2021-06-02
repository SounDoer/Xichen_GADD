# Topic Candidates

Make Far Cry 6 CD Disc Gun with Unreal Engine 5 MetaSounds

Ambience Sound Design  
Location-Based or Player-Centered

From Channel-Based To Object-Based  
Cinematic Sound Implementation and Mixing with Level Sequence in UE

Why no sound?!

I/O Streaming

Sound Smooth Stop at Level Change

General Audio Structure and Naming Convention


如何面对“交互媒体本身的不确定性”和“工作流程依赖关系引起的不确定性”引起的“重复劳动问题”

游戏音频设计在整个游戏开发环节中通常处于 Dependency 最少的环节，因此只有开发自己项目的经历中能从各个具体的例子中真正地体会到某些问题：
1. 为什么需要迭代？游戏开发是复杂的系统工程，互相牵绊，想要一步到位实现最终声音/整体效果是不太可能的
2. 具体的表现形式决定了代码实现的方式，所以在进行实际代码工作前需要想清楚具体的表现效果
3. 大多数声音的触发其实只是一行代码，因此在功能需求大致完成之后再实现表现是合理的（可能这也是为什么声音需求优先级不太高的原因）

The Same Goal of Game Audio Design and Film Sound Design in different path  
学校教育仍然停留在传统的声音设计思路上，并没有讲清楚游戏音频的设计思路特殊性在哪里  
先说浅显易懂的非线性和交互性  
这几年去学校讲课 intro，经常要说到游戏音频设计与传统声音设计的不同之处，常说的几点就是非线性、交互性等之类的，但这些只是实现手段上的不同而已，从观众或玩家的角度来看，最终想要达到的目的其实是一致的  
游戏和影视本都属于视觉媒体声音设计范畴  
从空间上/最终要实现的效果上考虑……  
游戏音频设计与影视声音设计的殊途同归，一个在 DAW，一个在引擎
为什么 Dolby Atmos  
Channel-Based 声音设计逻辑终将淘汰，Object-Based 声音设计逻辑才是未来  

影视在做的事，从制作环节引入 Spatial Audio，Dolby Atmos，不断脱离 Channel-Based 的限制……  
而游戏做的则是在迁就 Channel-Based……

结合游戏引擎的电影制作方式……现场同期/动捕现场，Object-Based Sound Capture

殊途同归的现实举例就是 VR，论文
用VR游戏和VR影视距离，基于引擎和基于DAW的制作方式，都是为了实现同样的目的，更全面的沉浸感

现在是否是好时机，消费级支持 Spatial Audio 的产品越来越多……

声音素材还是基于 Channel 的，怎么解