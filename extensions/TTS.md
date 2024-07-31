# TTS

SillyTavern 提供了多种 TTS 选项。本页面解释了设置和使用方法。

### 什么是它？

TTS用于让声音朗读您聊天的部分内容。

### 配置 TTS

#### TTS 提供者选择框

用于选择您想要使用的 TTS 服务。

- **ElevenLabs** - 需要付费订阅，目前提供最高质量的声音。
- **Silero** - 免费，在您的 PC 上运行，质量可能差异很大。
- **System** - 使用您的操作系统 TTS 引擎（如果存在）。质量可能因操作系统而异。
- **Edge** - 免费，通过 Azure 运行，通常速度较快，声音听起来自然但干燥且没有情感。就像在听晚间新闻或广播播音员。
- **Coqui-TTS** - 免费，目前没有 API 实现。高性能的 Text2Speech 模型（Tacotron, Tacotron2, Glow-TTS, SpeedySpeech）以及 Bark。
- **Novel** - 需要付费的 NovelAI 订阅，由 NovelAI 的 TTS 引擎生成。
- **RVC** - 免费，声音克隆。

#### 复选框

- **Enabled** - 开启/关闭 TTS 播放。
- **Auto Generation** - 当新消息进入聊天时，自动开始播放 TTS。
- **Only narrate "quotes"** - 限制 TTS 播放仅包括 `"引号"` 内的文本。这将 `*包括 "引号" 在星号行内*`（内部变量名 = `narrate_quoted_only`）。
- **Ignore \*text, even "quotes", inside asterisks\*** - TTS 将不会播放任何 `*星号*` 内的文本，甚至 "引号"（内部变量名 = `narrate_dialogues_only`）。
- *同时选中 "仅叙述引号" 和 "忽略星号" 复选框将导致 TTS 仅读取不在星号内的 "引号"，并忽略其他所有内容。*
- **Narrate only the translated text** - 这将使 TTS 仅叙述翻译后的文本。

给定示例文本：`*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.` 
以下是根据 **Ignore \*text, even "quotes", inside asterisks\*** 和 **Only narrate "quotes"** 的布尔状态修改文本的表格：

| **Ignore \*text, even "quotes", inside asterisks\*** 	| **Only narrate "quotes"**	| **输出**                                                                   |
| :---------------------------------------------------- | :------------------------ | :--------------------------------------------------------------------------- | 
| Disabled                                              |	Disabled	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says.    |
| Disabled                                              | Enabled	                  | "nya"... "Good evening, senpai"                                              |
| Enabled	                                              | Disabled	                 | "Good evening, senpai", she says.                                            |
| Enabled	                                              | Enabled	                  | "Good evening, senpai"                                                       |

#### 滑块

这些将根据您选择的 API 进行更改。

（解释即将到来）

#### 按钮

- **Apply** - 在设置 TTS API 和编辑语音映射后必须点击此按钮。
- **Available voices** - 加载一个弹出窗口，显示您选择的 API 可用的所有声音，并允许您通过示例对话预览它们。

### 使用 TTS

1. 点击“启用”复选框，否则将不会有任何反应。
2. 如果希望每次新消息到达聊天时 TTS 自动启动，请点击“自动生成”复选框。
3. 可选地，点击任何消息右上角的扬声器图标以按需播放。
4. 点击右下角的“停止”按钮（在魔杖菜单中找到）以停止任何播放。

#### 声音映射

您必须提供一个声音映射供 TTS 使用，否则它将不知道每个角色应该使用什么声音。

这些必须遵循以下确切格式：

`CharacterName:TTSVoice,CharacterName2:TTSVoice2`

对于 Coqui-TTS，格式需要包括来自 WebGUI 的说话者和语言：

`CharacterName:TTSVoice[speakerid][langid]`
或
`Aqua:tts_models--multilingual--multi-dataset--your_tts\model_file.pth[2][1]`

#### Bark ZeroShot 声音克隆说话者

如果使用 Bark，您必须创建一个包含声音文件的声音文件夹以进行克隆。确保将声音添加到 homedir\tts\bark_v0\speakers\。在 Windows 上，路径可能是 C:\Users\USERACCOUNT\AppData\Local\tts\bark_v0\speakers\，在文件资源管理器中输入 %appdata% 然后向上一级目录查找 local，您应该会看到 tts。

目录应如下所示：
- homedir
  - tts
    - bark_v0
      - speakers
        - customvoice1
          - speaker.wav
          - speaker.npz
        - robinwilliams
          - speaker.mp3
        - me
          - speaker.mp3

首次加载此模型和声音时，Bark 将克隆声音并创建一个 .npz 文件，这对于更快的 TTS 是必需的。