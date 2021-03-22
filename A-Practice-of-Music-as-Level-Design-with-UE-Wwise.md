# “音乐作为关卡设计” UE & Wwise 实践案例

2021年1月 Global Game Jam，我与叶梓涛做了一款着重声音体验的禅意动作游戏[《剑入禅境 Sword Zen》](https://yezi.itch.io/sz)。受限于 GameJam 开发时间，当时的设计想法并没有能够彻底实现；不少玩家朋友们试玩之后发来了反馈，也让我们有了新的灵感。因此正好把这个项目作为个人的游戏开发练习，我开始从核心声音体验和玩法扩展性的角度入手重新编写了代码。\
在完成了原有玩法的重构之后，将要进行的一个最大的设计改动就是要让玩家的操作与游戏中的音乐产生更强的关联。音乐不再只是游戏进度的提示和主观情绪的表达，而是提供了更类似于音乐节奏游戏中“音乐作为关卡设计”的功能。比如，通过音乐中的节拍点来控制敌人的攻击行为，这样的话玩家作出相应反馈时的格挡成功与失败的时间点都会统一在音乐整体的节奏中，给玩家一种类似音游的打击爽快感；同时，这也为之后互动音乐的设计提供了更多可能性，根据玩家单次或多次的格挡结果来顺畅地衔接不同的音乐片段，引导玩家情绪和切换游戏阶段。\
接下来我将使用游戏引擎 Unreal Engine 和音频中间件 Wwise 工具，以“音乐节拍信息控制敌人攻击行为”为例，来详细分析一下设计思路和代码实现的具体过程。
```
开发环境与工具：
Unreal Engine 4.26 C++ & Blueprint
Audiokinetic Wwise 2019.2.9
```

### 设计需求图解

![HandleAttack Function Explanation](media/MusicAsLevelDesign_HandleAttack.jpeg)

首先需要明确具体的设计需求。如上图所示，敌人的攻击行为由带有一个参数的 HandleAttack(WeaponType) 函数控制，WeaponType 决定了所用武器的各类属性，其中与此处设计相关的数值是 Charge Time，即不同武器有着不同的充能时间。HandleAttack 函数执行时首先获取当前的 WeaponType，然后进行 Start Charge，在经过 Charge Time 之后才实施 Actual Attack。需要实现的设计需求是，敌人在使用不同武器时（即 Charge Time 可变的情况下），Actual Attack 实施的时间点都将与音乐中的节拍点保持一致。\
其实在 Wwise 中通过音乐内的 Cue 或者 Marker 标记信息来触发函数的功能实现并不复杂，原生 API 中的 PostEvent() 函数本身就开放了回调函数（Callback Function）。**只不过，这个案例的设计需求稍微有些不同的地方是，音乐中需要标记的时间点确实是在节拍点上，但实际调用函数的时间点却是在标记的节拍点之前，且调用函数的提前时间量是由游戏中的参数来实时决定的，因此每次实施攻击时都需要进行额外的计算。**

### 尝试使用 Wwise Trigger 功能

![Wwise Trigger Solution](media/MusicAsLevelDesign_WwiseTriggerSolution.jpeg)

既然已经使用了 Wwise，

对 Trigger 功能的两点建议
1. API 改进
2. Trigger 功能通用化

### New Solution

### Thought

更大的可能性
Game Flow 都由音乐来控制

#### Reference

[Wwise SDK - Integration Details - Events](https://www.audiokinetic.com/library/edge/?source=SDK&id=soundengine_events.html)\
[Wwise SDK - Integration Details - Music Callbacks](https://www.audiokinetic.com/library/edge/?source=SDK&id=soundengine_music_callbacks.html)\
[Wwise SDK - Integration Details - Triggers](https://www.audiokinetic.com/library/edge/?source=SDK&id=soundengine_triggers.html)


希辰\
2021.3.20

***