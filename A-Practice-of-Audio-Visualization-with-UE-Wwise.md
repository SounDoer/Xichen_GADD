# “音频可视化” UE & Wwise 实践案例

继上一篇文章[《“音乐作为关卡设计” UE & Wwise 实践案例》](https://github.com/SounDoer/Xichen_GADD/blob/main/A-Practice-of-Music-as-Level-Design-with-UE-Wwise.md)中运用音乐中的标记信息来触发游戏事件之后，我想在个人练习项目里想实现的下一个功能是用音乐中的信息来驱动视觉效果的表现，也就是通常说的音频可视化（Audio Visualization）。\
音频可视化本身是一个大课题，在数字艺术和现场表演等领域已经有了成熟的工具和艺术表达。本文不讨论艺术方面的表现效果，也不深究音频信号的数理定义，仅从基本概念入手，结合引擎工具 Unreal Engine 和音频中间件 Wwise 已有的功能，来实现一些基本的音频可视化效果。

### 音频可视化的要素

对于数字音频信号的分析，通常可以从时域（Time Domain）和频域（Frequency Domain）两个方面入手。

### 现有工具的局限

### 具体的实现思路

UE 原生功能组件
Wwise 局限

#### 时域：声音包络（Envelope）

一维的连续变化的数据
Envelope / Amplitude / Magnitude
Weighted Loudness

#### 频域：声音频谱（Spectrum）

一维数组
Frequency
Spectrum

#### 时域与频域：声音时频谱（Spectrogram）

Spectrogram

### 更进一步的代码实现


#### Reference
[UE4 / Unreal Engine 4 / Audio Visualiser - #01 Dancing Cube Array.](https://www.youtube.com/watch?v=ix3oa7nB2VA)


希辰\
2021.4.3

***