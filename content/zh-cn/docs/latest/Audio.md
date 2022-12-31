---
title: 音频
menu:
  docs:
    parent: timing
weight: 5000
---

毫不夸张的说，Aegisub有着很先进的、可定制的音频模式，它既能显示波形也能显示频谱。
有几种方式可以进行整句时间轴编辑和卡拉OK时间轴编辑。

## 打开音频

想要将音频读取到Aegisub内，只需要点击音频 *音频* 菜单，然后点击
*打开音频文件* 。如果你已经读取了视频
(视频文件内含有音频轨道)，你可以使用 *从视频中打开音频*
，它会从当前读取的视频文件中读取音频。你可以打开任何音频文件，如果
[音频来自]({{< relref "Options#advancedaudio" >}})) 中设置的音频源可以解码。
另外，如果你不想使用真实的音频文件，你也可以选择 *打开150min空白音频*
或者 *打开150min噪音*.

如果打开音频失败时的错误提示提到了音频播放器，或者播放音频时没有声音，请参阅调整
[不同的音频播放器]({{< relref "Options#advancedaudio" >}}))。

### 支持的格式

Aegisub 一般情况下使用 [FFMS2](https://github.com/FFMS/ffms2)
来打开音频，
它很可靠，基本可以打开你能想到的所有音频格式。在Windows下，Aegisub也能使用DirectShow
(通过Avisynth)，它速度会快一些，因为它不必在打开音频文件之前进行索引。然而，DirectShow不是很可靠(事实上是很不可靠)，如果你通过Avisynth调用DirectShow打开一个多音轨的音频文件，可能会发生十分令人心塞的事情。

Aegisub只支持单声道音频。多声道的音频在导入时会自动混为单声道，如果音频文件的声道数多于两个，结果可能悲剧。

### 音频缓存

如果你读取的音频类型不是未压缩的WAV格式
(PCM)，Aegisub会先转码然后缓存它。读取的时候也会混为单声道(通过调节
[音频 downmixer 选项]({{< relref "Options#avisynthwindowsonly" >}})
你也可以挑选想听的声道)，把文件解压成 PCM (就是WAV)，然后(默认情况下)
读取到内存中。这意味着你需要 *较大的*
内存才能读取长度较长的压缩音频。如果内存不足或者你在处理整部电影，参考
[音频缓存设置]({{< relref "Options#cache" >}})使Aegisub使用硬盘(速度慢)作为缓存区域；或者你可以在读取之前把压缩音频转成WAV格式，因为Aegisub直接读取WAV是不需要建立缓存的。

下面为Aegisub读取音频占用内存的计算公式: s = ( b * r * l ) / 8 *s*
是需要占用的内存 (byte单位 - 足1024进1kB)； *b* 代表音频文件的位深度
(最常见的是16bit音频，当然也有更高的bit，主要影响音频的动态范围)； *r*
是指音频文件的采样率 (Hz为单位，通常为48000，或者44100)； *l*
是指音频文件的长度 (秒为单位).

举例，一个25分钟的采样率为48 kHz的音频片段，你需要大约(16 * 48000 * 25
\* 60)/8 = 144000000 bytes ~= 137 MB内存。

读取和解压音频需要一些时间。
读取音频时会显示进度条(译者注：在新版本中从视频中读取时不会出现进度条，取而代之的是音频框中的进度)。