# AllTalk TTS

AllTalk 是一个基于 Coqui XTTS 模型的语音克隆系统，只需一个高质量的 wav 音频文件即可尝试重现原始说话者的声音。它还提供微调选项，以在需要时提高语音的重现质量。

要查看完整的功能集，请访问此 [link](https://github.com/erew123/alltalk_tts?#alltalk-tts) 和 [Screenshot here](https://github.com/erew123/screenshots/raw/main/sillytavern.jpg)

除了标准的 SillyTavern 文本分割选项以选择说出或不说出的内容外，AllTalk 还提供了额外的选项，可以使用 AllTalk 叙述者，以便以替代声音朗读非角色文本 [example](https://vocaroo.com/18nrv7FR6wuA)

文档和自定义设置内置于 AllTalk，并可通过 SillyTaven TTS 界面中的链接获取。

## 🟩 设置

您有 2 种当前的安装选项。如果您使用 Text-generation-webui 作为 LLM 模型加载器，可以将 AllTalk 作为扩展添加。如果您希望将 AllTalk 作为独立使用，也可以这样做。有关最新的文档、支持和帮助，请访问 [这里](https://github.com/erew123/alltalk_tts?#alltalk-tts)。以下是两种快速安装方法（附视频），如果您需要手动安装方法，请查看 AllTalk 网站。

点击展开以下其中一个：

<details>
	<summary>快速设置 - Text-Generation-webui</summary><br>

   **注意** 您需要在 **Text-generation-webui 界面**中 **取消选中** “启用 TTS”，否则在使用 SillyTavern 时，由于 Text-generation-webui 发送消息的方式，AllTalk 将双重生成 TTS。可以在 AllTalk 设置页面将其设置为启动设置。

 如果您希望查看视频，请访问 [这里](https://www.youtube.com/watch?v=icn2XS5rUH8)
1) 要下载 AllTalk 设置，您可以选择：
   - A) 在 AllTalk [页面](https://github.com/erew123/alltalk_tts)选择 **代码** > **下载 ZIP**，然后将其解压到 text-generation-webui 扩展文件夹中<br>例如 `\text-generation-webui\extensions\alltalk_tts\`<br><br>
   - B) 打开终端/控制台，进入 `\text-generation-webui\extensions\` 文件夹<br>并 `git clone https://github.com/erew123/alltalk_tts`<br><br>
3) 在终端/命令提示符中，在 text-generation-webui 文件夹中，您将使用 `cmd_windows.bat` 或 `./cmd_linux.sh` 启动其 Python 环境
4) 进入 AllTalk 文件夹，例如 `cd extensions` 然后 `cd alltalk_tts`
5) 启动 AllTalk 设置脚本 `atsetup.bat` 或 `./atsetup.sh`
6) 按照屏幕上的提示安装所需的正确要求文件。建议在安装 DeepSpeed 之前测试 AllTalk 是否正常工作。
7) 当 AllTalk 服务器启动时，其默认设置和文档页面将在 http://127.0.0.1:7851/

   每次需要对 AllTalk 进行更改，或使用微调等功能时，请始终先启动 Text-generation-webui Python 环境。

   请阅读 `🟩 其他安装说明`（那里还有额外的声音可用）。

   微调和 DeepSpeed 有其他安装要求（根据您的操作系统），因此请阅读设置工具中的任何说明，并参考 AllTalk Github 页面以获取详细说明。<br><br>
</details>

<details>
	<summary>快速设置 - 独立安装</summary><br>

 如果您希望查看视频，请访问 [这里](https://www.youtube.com/watch?v=AQYCccDRbaY)
1) 要下载 AllTalk 设置，您可以选择：
   - A) 在 AllTalk [页面](https://github.com/erew123/alltalk_tts)选择 **代码** > **下载 ZIP**，然后将其解压到您选择的文件夹中<br>例如 `C:\myfiles\alltalk_tts\`<br><br>
   - B) 打开终端/控制台，进入您选择的文件夹，例如 `C:\myfiles\` 文件夹<br>并 `git clone https://github.com/erew123/alltalk_tts`<br><br>
4) 在终端/命令提示符中，进入 AllTalk 文件夹，例如 `cd alltalk_tts`
5) 启动 AllTalk 设置脚本 `atsetup.bat` 或 `./atsetup.sh`
6) 按照屏幕上的提示安装所需的正确要求文件。建议在安装 DeepSpeed 之前测试 AllTalk 是否正常工作。
7) 当 AllTalk 服务器启动时，其默认设置和文档页面将在 http://127.0.0.1:7851/

   Windows 机器上的 DeepSpeed 将作为标准安装。Linux 机器有其他要求，这些要求在设置工具和此页面中有详细说明。

   请阅读 `🟩 其他安装说明`（那里还有额外的声音可用）。

   微调有其他安装要求，因此请阅读设置工具中的任何说明，并参考 AllTalk Github 页面以获取详细说明。<br><br>
</details>

#### 🟩 其他安装说明
在首次启动时，AllTalk 将下载 Coqui XTTSv2 2.0.2 模型到其 **models** 文件夹（需要 1.8GB 的空间）。如果您想知道它在做什么，请检查命令提示符/终端窗口。在显示“模型已加载”后，AllTalk 通常会在几秒钟后在其 IP 地址上可用，您可以在浏览器中连接。

扩展加载后，请在 http://127.0.0.1:7851/ 上查找所有文档和设置。

**在哪里可以找到声音** https://aiartes.com/voiceai 或 https://commons.wikimedia.org/ 或 YouTube 上的访谈等。有关如何剪切和准备声音样本的说明在内置文档中。

请阅读有关确保您的角色卡正确设置的说明。

一些额外的 AllTalk 声音可以在 [这里](https://drive.google.com/file/d/1bYdZdr3L69kmzUN3vSiqZmLRD7-A3M47/view?usp=drive_link) 下载。

#### 🟩 关于角色卡和问候消息的说明
为叙述者准备的消息应放在星号 `*` 中，而为角色准备的消息应放在双引号 `"` 中。然而，AI 系统经常偏离这些规则，导致文本既不在引号中也不在星号中。有时，文本可能只带有一个星号，AI 模型可能在对话中间改变其格式。例如，它们可能最初使用星号，然后切换到未标记的文本。正确格式化的行应如下所示：

`"`嘿！我很高兴终于见到你。我听说过很多关于你的好事，我迫不及待想和你讨论计算机。`"` `*`她走过房间，拿起了她的咖啡杯`*`

大多数叙述者/角色系统在遇到星号或引号时会切换声音，这在一定程度上是有效的。AllTalk 在其句子拆分和识别方法上经过了几次修订。虽然在消息格式中一些不规则性和 AI 偏差是不可避免的，但现在任何以星号开头或结尾的行应该被识别为叙述者对话。用双引号括起来的行被识别为角色对话。对于任何其他文本，您可以选择 AllTalk 如何处理它：无论它应该被解释为角色还是叙述者对话（大多数 AI 系统在生成未用引号或星号括起来的文本时倾向于更多地偏向某种格式）。

您可以在终端/命令提示符中监控 AllTalk 识别的叙述者行，并在需要时调整其行为（文本不在 - 功能）。

## 🟨 在 SillyTavern 中使用 AllTalk 及理解设置

当您加载 AllTalk 后，在 SillyTavern 的 TTS 页面中选择 AllTalk，底部有一个链接 "AllTalk Config & Docs"。点击该链接，它将带您到您的 AllTalk 服务器，在那里您可以找到设置的解释。

某些设置不能同时使用，因此 AllTalk 会根据需要启用/禁用某些设置。

如果在 AllTalk 可用之前加载了 SillyTavern，您可能需要在 SillyTavern 的 TTS 扩展页面上点击重新加载按钮。

在 SillyTavern 中使用 AllTalk 的一些快速提示：

- 每次只更改 DeepSpeed、Low VRAM 或 Model 中的一个。等待它显示“准备就绪”后再更改其他内容。
- 您可以在 AllTalk 设置页面中永久更改 AllTalk 启动设置或 DeepSpeed、Low VRAM 和 Model。
- 不同的 AI 模型对引号和星号的使用方式不同，因此在使用 AllTalk 叙述者时，您可能需要根据模型更改“文本不在内部”。
- 将新的语音样本添加到 voices 文件夹。您可以对模型进行微调，使其听起来更接近原始样本。
- DeepSpeed 将使 TTS 的处理时间提高到 2-3 倍更快（在独立安装中默认安装，但需要激活）。
- 如果在加载 LLM 后剩余内存不多，Low VRAM 会非常有益。

## 🟨 支持、帮助、协助及其他问题

请访问 [这里](https://github.com/erew123/alltalk_tts?#alltalk-tts)