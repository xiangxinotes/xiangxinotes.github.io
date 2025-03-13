---
layout: post
title: 「2025最新」Mac 电脑录屏的时候，怎样才能只录制电脑发出的声音呢？（详细教程）
date: 2025-03-13 20:15:35 +0800
description: Mac 录屏麦克风选“无”录不到任何声音，选“内置麦克风”又把电脑发出的声音和环境音都录制进去了。用“截屏” App 和 QuickTime Player 都不能只录制电脑发出（扬声器）的声音，Soundflower 停止开发，而且苹果在 macOS Catalina（10.15）之后也不再支持。那怎么只录制扬声器的声音，而不录到环境音呢？本文包含一步一步的行动指南，助你成功录屏。
image: set-up-multi-output-device-to-record-just-the-speaker.png
tags: mac backhole guide tech soundflower
---

我们都知道[怎么用 Mac 电脑录屏](https://support.apple.com/zh-cn/102618)，但是我们也都面临了同一个问题：Mac 录屏麦克风选“无”的话，录不到任何声音。但是选“内置麦克风”又会把电脑发出的声音（扬声器）和环境音都录制进去了，杂音太大。

今天，你，就是你🫵，你走运啦！

我花了那么多时间，研究网上的各种教程，设置、Soundflower 都试过不行，今天终于成功在录屏的时候**只录到扬声器声音**（不录环境音）了。

接下来，你只需要花 6 分钟阅读本文就可以解决 Mac 录屏的声音问题啦。本文将为你提供一份详细的 step by step 指南，助你轻松搞定此事。

只需 5 步，简单快捷：
1. 下载 BlackHole
1. 安装 BlackHole
1. 打开 “音频 MIDI 设置”
1. 设置音频设备
1. 将系统的音频输出设备设置为此音频设备

点击文章目录可以快速跳转到对应内容哦～

## 步骤指南

### 1. 下载 BlackHole

BlackHole 是一款免费开源软件，所以你可以在 Github 上找到 [BlackHole 的开源项目](https://github.com/ExistentialAudio/BlackHole)。如果你想要了解这个项目，可以去看一看哦。

BlackHole 安装包的下载链接：[https://existential.audio/blackhole](https://existential.audio/blackhole)

下载页面操作指南：

如果你十分感激作者，可以点击 "Donate $10" 按钮支持作者，当然你也可以点击 "I can't afford to donate" 按钮。

![下载链接中包含2个按钮："Donate $10" 和 "I can't afford to donate"]({{ site.baseurl }}/images/retrive-blackhole-download-link.png)
*打开下载链接后会出现2个按钮："Donate $10" 和 "I can't afford to donate"*

点击 "I can't afford to donate" 按钮后，将会需要你填写你的邮箱及姓名。信息填写完毕，你将会收到一封邮件：**BlackHole Download**（如果收件箱里没看到，可以查看垃圾箱）。

![邮件内容：Thank you for downloading. Please click the following link to download BlackHole: Virtual Audio Driver. The link will be active for 24 hours. 以及下载链接]({{ site.baseurl }}/images/email-detail-of-the-blackhole-donwload-link.png)
*你将会收到的邮件内容如上*

点击邮件中的链接，注意链接有效期为 24 小时。有 3 个下载器供选择，如果不知道下载哪一个，就下载 **BlackHole 2ch**

![页面中有 3 个下载器供选择：BlackHole 2ch, BlackHole 16ch, BlackHole 64ch]({{ site.baseurl }}/images/three-backhole-installers-to-choose-from.png)
*有 3 个下载器供选择：BlackHole 2ch, BlackHole 16ch, BlackHole 64ch*

### 2. 安装 BlackHole

双击在第1步中下载的 pkg 安装包（我的安装包叫 BlackHole2ch-0.6.1.pkg）来安装它。按照步骤及提示完成安装操作。

### 3. 打开 “音频 MIDI 设置” App

如果你不知道这款 App，可以 Command + 空格来搜索它。

你会注意到左侧有 3 个声音设备：
- 一个是🎤麦克风，
- 一个是🔉扬声器，
- 另一个是你安装的 BlackHole（比如我的就是 BlackHole 2ch）。

![“音频 MIDI 设置” App 界面左侧有 3 个声音设备：麦克风、扬声器、BlackHole]({{ site.baseurl }}/images/sceenshot-of-audio-midi-setup.png)
*“音频 MIDI 设置” App 界面左侧有 3 个声音设备：麦克风、扬声器、BlackHole*

### 4. 设置音频设备

为了只录制电脑发出的声音，也就是只录制扬声器的声音，我们需要添加一个新的音频设备。

点击 “音频 MIDI 设置” App 界面左侧下方的加号+，在弹出的菜单里选择 “**多输出设备**” ，并完成下图配置：

![“多输出设备”的页面设置：主要设备选择扬声器；扬声器和 Blackhole 都要勾选 Use；音频设备的顺序为扬声器第一，Blackhole 第二；扬声器不必勾选 Drift Correction，但 Blackhole 请勾选上 Drift Correction。]({{ site.baseurl }}/images/set-up-multi-output-device-to-record-just-the-speaker.png)
*此“多输出设备”的页面设置*

⚠️ 注意：
- 主要设备请选择**扬声器**；
- 扬声器和 Blackhole 都要勾选 **Use**；
- 音频设备的顺序为**扬声器第一**，**Blackhole 第二**；
- 扬声器不必勾选 Drift Correction，但 **Blackhole** 请勾选上 **Drift Correction**。

如果音频设备默认顺序为扬声器第 2，且已勾选 Use，那再点一次取消 Use，然后依次勾选扬声器、Blackhole 即可。

### 4. 将系统的音频输出设备设置为此音频设备

完成此音频设备的设置后，最重要的一步就是在此音频设备上右键，选择“**使用此设备进行声音输出**”，之后你就可以开始录屏了。

## Blackhole 的缺点

我已经测试过了，完成以上步骤后只会录到电脑扬声器的声音。

但 Blackhole 有个缺点，那就是：更改系统的音频输出设备之后，键盘上的**音量调节键**就失效了。对此官方给出的解决方案是：点击 “音频 MIDI 设置” App 中的**扬声器**进行音量调节。

所以如果你录屏的时间不长，次数不多，那 Blackhole 用起来就比较麻烦，你可能需要不停地切换系统的音频输出设备。方法就是：
- 正常使用时，在扬声器上右击，选择“使用此设备进行声音输出”；
- 录屏的时候，调整好扬声器音量，然后在设置好的音频设备上右击，选择“使用此设备进行声音输出”。

你能接受这个缺点吗？如果不能接受的话，请继续寻找更好的解决方案，找到了也可以在评论区分享给我，谢谢！那我们就下一篇文章再见喽！👋

---
