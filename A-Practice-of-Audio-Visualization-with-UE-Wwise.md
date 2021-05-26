# “音频可视化” UE & Wwise 实践案例

继上一篇文章[《“音乐作为关卡设计” UE & Wwise 实践案例》](A-Practice-of-Music-as-Level-Design-with-UE-Wwise.md)中运用音乐中的标记信息来触发游戏事件之后，我想在个人练习项目里想实现的下一个功能是用音乐中的信息来驱动视觉效果的表现，也就是通常说的音频可视化（Audio Visualization）。  
音频可视化本身是一个大课题，在数字艺术和现场表演等领域已经有了成熟的工具和艺术表达。本文不讨论艺术方面的表现效果，也不深究音频信号的数理定义，仅从基本概念入手，结合引擎工具 Unreal Engine 和音频中间件 Wwise 已有的功能，来实现一些基本的音频可视化效果。

```
开发环境与工具：
Unreal Engine 4.26 C++ & Blueprint
Audiokinetic Wwise 2021.1.0
```

### 音频可视化的元素

对于数字音频信号的分析，通常可以从**时域**（Time Domain）和**频域**（Frequency Domain）两个方面入手，相关的分析工具和表现形式在各类播放器和音频工具中其实都很常见。

简单来说，从时域上分析可以得知音频信号的能量大小随时间变化的情况，根据不同的计算与加权方式可以表示各种与能量相关的数值情况。比如，峰值（Peak）表现在声音波形（Waveform）上就是波形最外层的包络曲线（Envelope），响度（Loudness）表现在表头上就是实时变化的数值指示。

![Waveform & Meter](media/AudioVisualization_WaveformAndMeter.png)

从频域上分析可以得知某一时刻音频信号的能量在频率上的分布情况，即常见的频谱图（Spectrum）所表示的各个频段在人耳可闻频谱范围内的能量大小。

![Spectrum](media/AudioVisualization_Spectrum.png)

除此之外，还有一种将时域和频域信息整合在一起的可视化方式，即时频谱（Spectrogram），3D 形式的时频谱也因其立体形象而被叫做“瀑布图”。

![Spectrogram](media/AudioVisualization_Spectrogram.png)

下文将以实现上述三种可视化效果来展开。

### 结合现有工具的实现思路

由于项目已有的音频功能都是基于中间件 Wwise 的，音频可视化的实现也需要统一在同一框架下，因此具体的实现方式也多了一些限制和要求：  
1. 由于编程水平有限，尽量不考虑自己修改或重写代码的方式来实现；  
2. 据我了解，除了 Wwise Meter 插件之外，目前 Wwise 并没有直接与音频分析相关的功能，无法从 Wwise 中便捷地获取频谱相关的声音信息；  
3. UE 引擎确实有不少与音频分析相关的功能，但需要在其自身的音频系统框架下使用，这样一来，声音资源的管理和控制都将与 Wwise 脱节，因此也不适用。

![UE Blueprint Audio Visualization](media/AudioVisualization_UE_Blueprint_Overview.png)

所以，一种可行的实现思路是，利用 UE 引擎中的功能对声音文件进行分析并存储相应的数据，然后通过 Wwise 控制声音的播放并调用对应时刻的信息。

### 时域：声音包络（Envelope）

首先从相对简单的 Envelope 入手。从数据类型来看，音频信号在时域上的瞬时值可以理解为是一个随时间快速变化的浮点数变量，可以直接用来驱动视觉元素的各种参数；更进一步，可以将一段时间内的变量数值先存储进数组，然后用数组来驱动视觉元素的变化，这样就能表现出音频信号在一段时间内的 Envelope 形态。

如上一节实现思路中所提到的，想尽可能在 Wwise 框架下实现功能，而 Wwise Meter 插件正好可以满足这一要求。在中间件内 Audio Bus 上使用 Wwise Meter 插件来获取音频信号的时域瞬时值，然后映射到创建的 Game Parameter 上，最后在引擎中通过 GetRTPCValue 节点来获取该数值。

![Wwise Meter RTPC](media/AudioVisualization_Envelope_Wwise_Meter_RTPC.png)

在引擎中获取到该数值后，首先通过 MapRangeClamped 和 FInterpTo 节点对数据进行标准化和平滑处理。然后，将数据存入一个定长的浮点数数组中，数组长度可以根据需要展示的 Envelope 长度来决定。更新数组内数据的思路是，在每一次 Tick 时，最新的数值永远存储在数组的最开头位置，数组之后的位置依次复制前一位的数值，这样就能将一段时间内的数值变化存储下来并且持续进行更新。如果以每秒 60 帧的 Tick 速率来计算，长度为 60 的数组可以记录 1 秒内的数值变化。最后，就可以用该数组内的数据来同步驱动视觉元素的表现了。

![UE Update Envelope Line](media/AudioVisualization_Envelope_UE_UpdateEnvelopeLine.png)

Envelope 效果如下动图。

![Audio Visualization Envelope Demo](media/AudioVisualization_Envelope_Demo.gif)

### 频域：声音频谱（Spectrum）

从数据类型来看，音频信号在某一时刻的频谱信息可以理解为是一个浮点数数组，数组长度代表了频率范围，而其中各个位置上的数值可以看作是时域瞬时值在频域上的展开。用该数组驱动视觉元素的变化并不断更新，就可以表现音频信号的 Spectrum 形态。

目前 UE Audio Synesthesia 组件提供了三种音频分析工具 LoudnessNRT、ConstantQNRT 和 OnsetNRT，分别可以提取音频信号的响度、频谱和瞬态变化的信息。其中 NRT 的意思是非实时（Non Real Time），工作原理是将 Wav 音频文件导入引擎中并配置分析工具，分析得到的数据会被保存在相应的 NRT 对象中，获取当前音频信号的播放位置即可从 NRT 对象中读取对应时刻的信息。  
因此，可以使用 ConstantQNRT 分析工具来获取并保存音频信号的频谱信息。其中 ConstantQ 指的是信号处理中的[常数Q变换](https://en.wikipedia.org/wiki/Constant-Q_transform)，与常见的傅里叶变化有关。具体有何区别我也讲不清，反正可以用来获取声音的频域信息就对了。

如下图，首先在引擎中创建 ConstantQNRT 对象，并在其中配置需要分析的音频文件。有需要的话，还可以创建 ConstantQNRT Setting 对象，进一步设置与频谱分析相关的各种参数。

![UE Audio Synesthesia](media/AudioVisualization_Spectrum_Synesthesia_ConstantQNRT_Object.png)

接下来，使用引擎原生的 Audio Component 来设置播放的音频文件并获取其时长，然后通过 OnAudioPlaybackPercent 事件来获取 Playback Percent，该数值与音频文件时长相乘即可得到以秒为单位的 Current Play Position，最后使用 GetNormalizedChannelConstantQAtTime 节点来读取 ConstantQNRT 中相应时刻的信息。

![UE Audio Synesthesia Native Audio Component](media/AudioVisualization_Spectrum_Synesthesia_NativeAudioComp.png)

以上，就是使用 UE 引擎原生组件来获取声音频谱信息的方式。如果要引入中间件 Wwise 来控制，只要能获取音频文件的当前播放位置就可以了。在[《“音乐作为关卡设计” UE & Wwise 实践案例》](A-Practice-of-Music-as-Level-Design-with-UE-Wwise.md)的“获取当前播放位置信息”章节中，讲解了创建 GetSourcePlayPosition 函数从 PostAkEvent 节点的 Callback 信息中获取当前播放位置的方法，在这里直接使用这一函数节点就能从 Wwise 控制的音频文件中获取当前播放位置，然后仍旧可以使用 GetNormalizedChannelConstantQAtTime 节点来读取 ConstantQNRT 中相应时刻的数组信息了，最后用此数据来同步驱动视觉元素的表现。

![UE Audio Synesthesia Wwise Control](media/AudioVisualization_Spectrum_Synesthesia_WwiseControl.png)

Spectrum 效果如下动图。

![Audio Visualization Spectrum Demo](media/AudioVisualization_Spectrum_Demo.gif)

### 时域与频域：声音时频谱（Spectrogram）

在实现了 Envelope 和 Spectrum 之后，再来理解 Spectrogram 就简单多了。Spectrogram 可以看作是由 Envelope 和 Spectrum 两者构成的一个二维数组，或者形象地来看就是一个二维矩阵，其中 Envelope 一轴表示的是时域，Spectrum 表示的则是频域。

获取当前播放位置和对应时刻频谱数据的方法与上一节 Spectrum 类似，主要区别在于如何用 Spectrum 数组信息来生成和驱动视觉元素二维矩阵的变化。如下图，通过 SpawnSpectrogramMatrix 和 UpdateSpectrogram 两个函数节点来实现。

![UE BP Spectrogram Overview](media/AudioVisualization_Spectrogram_UE_Overview.png)

在 SpawnSpectrogramMatrix 函数节点中，首先以 SpectrumBandNumber 为行长、SpectrogramLength 为列长来生成二维矩阵。其中 SpectrumBandNumber 对应的是从 ConstantQNRT 对象中获取到的单个 Spectrum 数据的数组长度，可以在 ConstantQNRT Setting 对象中设置；SpectrogramLength 表示的是同时展示的 Spectrum 数组的个数，数值越大则表现内容的时长越长。  
另外，在将生成的视觉元素存入数组的同时，需要将每一列最开始元素的索引值存入到 ColumnStartIndexArray 数组中，之后用来指示每一个 Spectrum 数组在矩阵中更新数据的起始位置。

![UE Spectrogram Matrix Initial](media/AudioVisualization_Spectrogram_SpectrogramMatrixInitial.png)

![UE BP Spawn Spectrogram Matrix](media/AudioVisualization_Spectrogram_UE_SpawnSpectrogramMatrix.png)

在 UpdateSpectrogram 函数节点中，遍历 ColumnStartIndexArray 数组并结合 SpectrumBandNumber 数值来找到每一列的起止范围；同时，以 CurrentPlayPosition 为基准、根据 ColumnStartIndexArray 中的索引值来对送入 GetNormalizedChannelConstantQAtTime 节点的 InSeconds 数值做偏差调整，这样就能从 ConstantQNRT 对象中获取一段时间内连续多个 Spectrum 数组的数据了。

![UE BP Update Spectrogram](media/AudioVisualization_Spectrogram_UE_UpdateSpectrogram.png)

Spectrogram 效果如下动图。

![Audio Visualization Spectrogram Demo](media/AudioVisualization_Spectrogram_Demo.gif)

### 总结

上述方法只是在文章开头提出的要求与限制下取巧地实现了音频可视化的功能，从各个方面来评判都算不上是最优解。一个显而易见的问题就是，同一个音频文件需要在引擎和中间件中分别导入，一份用于数据分析，一份用于播放控制，如果文件数量很多的话，就会造成存储资源的浪费。  
所以，如果没有需要配合中间件的要求，大可以直接使用 UE 引擎的原生功能来实现简单的音频可视化功能；当然理想情况是，Wwise 自身能有更多音频分析相关的功能和与引擎进行数据通信的方式。

### Reference

[Unreal Engine Blueprint API Reference - Audio Component](https://docs.unrealengine.com/en-US/BlueprintAPI/Audio/Components/Audio/index.html)  
[Unreal Engine Blueprint API Reference - Audio Analysis](https://docs.unrealengine.com/en-US/BlueprintAPI/Audio/Analysis/index.html)  
[Unreal Engine Blueprint API Reference - Audio Analyzer](https://docs.unrealengine.com/en-US/BlueprintAPI/AudioAnalyzer/index.html)  
[Unreal Engine Blueprint API Reference - Sound Visualization](https://docs.unrealengine.com/en-US/BlueprintAPI/SoundVisualization/index.html)  
[Unreal Engine Blueprint API Reference - Time Synth](https://docs.unrealengine.com/en-US/BlueprintAPI/TimeSynth/index.html)  
[Unreal Engine 4 Documentation - Audio Synesthesia](https://docs.unrealengine.com/en-US/WorkingWithMedia/Audio/Synesthesia/index.html)  
[Wwise Help - Wwise Meter](https://www.audiokinetic.com/library/edge/?source=Help&id=wwise_meter_plug_in_effect)

希辰  
2021.4.3

***