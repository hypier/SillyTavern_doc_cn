# 什么是 SillyTavern?

![SillyTavern - LLM 前端供高级用户使用](/static/banner.jpg)

由 Cohee、RossAscends 和 SillyTavern 社区共同推出，SillyTavern 是一个本地安装的界面，允许您与文本生成 AI（LLMs）进行互动，以聊天和角色扮演自定义角色。

SillyTavern 起源于 2023 年 2 月的 TavernAI 1.2.8 的修改版，并且自那时起添加了许多原始 TavernAI 中不存在的尖端功能。

### 更新后缺少扩展？

自 1.10.6 版本发布以来，大多数以前内置的扩展已转换为可下载的附加组件。您可以通过扩展面板中的内置“下载扩展和资产”菜单（位于顶部栏的堆叠块图标）下载它们。

### 截图

| ![VN 模式](/static/screenshot1.jpg) | ![角色管理](/static/screenshot2.jpg) |
|:---:|:---:|
| ![高级格式](/static/screenshot3.jpg) | ![斜杠命令](/static/screenshot4.jpg) |
| ![扩展](/static/screenshot5.jpg) | ![响应配置](/static/screenshot6.jpg) |
| ![背景](/static/screenshot7.jpg) | ![用户设置](/static/screenshot8.jpg) |

### 分支

SillyTavern 正在使用双分支系统进行开发，以确保所有用户的顺畅体验。

* release -🌟 **推荐给大多数用户。** 这是最稳定和推荐的分支，仅在推出重大版本时进行更新。适合大多数用户。
* staging - ⚠️ **不推荐用于日常使用。** 此分支具有最新功能，但请谨慎使用，因为它可能随时崩溃。仅适合高级用户和爱好者。

了解更多 [这里](https://docs.sillytavern.app/usage/branches/)。

### 功能

* 移动友好的界面
* 多个后端 API 连接 ([KoboldAI](https://github.com/KoboldAI/KoboldAI-Client), [KoboldCpp](https://github.com/LostRuins/koboldcpp), [AI Horde](https://horde.koboldai.net/), [NovelAI](https://github.com/LostRuins/koboldcpp), [Oobabooga's TextGen WebUI](https://github.com/oobabooga/text-generation-webui), [OpenAI](https://chat.openai.com/), [WindowAI](https://windowai.io), [OpenRouter](https://openrouter.ai/), [TabbyAPI](https://github.com/theroyallab/tabbyAPI) 等等...). 参见 [API 连接](https://docs.sillytavern.app/usage/api-connections/)。
* 类视觉小说的女友模式
* Horde 稳定扩散生成
* TTS 支持（ElevenLabs, Silero 等）
* WorldInfo（背景资料）
* 作者注释 / 角色偏见
* 可自定义颜色、背景、头像样式和 UI 面板位置
* AI 响应的通知声音
* 将聊天导出为 .txt 文件
* 通过 Google API 自动翻译单条消息或整个聊天，甚至可以自动进行。
* 广泛的提示格式选项
* 角色热交换按钮，快速切换您最喜欢的角色
* 每条消息的提示令牌分解视图
* 滑动
* 群聊：多机器人房间，让角色与您或彼此对话
* 聊天书签 / 分支

### 扩展

SillyTavern 支持扩展/插件：

* 角色情感表达
* 聊天历史的自动摘要
* 向聊天发送图片，并由 AI 解释内容。
* 稳定扩散图像生成（5 个与聊天相关的预设加上“自由模式”）
* AI 响应消息的文本转语音（通过 ElevenLabs、Silero 或操作系统的系统 TTS）

可以通过使用 [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-extras) 添加额外功能。 


### 截图

![](https://user-images.githubusercontent.com/18619528/228649245-8061c60f-63dc-488e-9325-f151b7a3ec2d.png)
![](https://user-images.githubusercontent.com/18619528/228649856-fbdeef05-d727-4d5a-be80-266cbbc6b811.png)

### 安装要求

* SillyTavern 可以在几乎任何能够运行 NodeJS v18 的设备上运行。
* 注意：由于上述要求，**SillyTavern 将无法在 Windows 7 上运行。**

### 除了 SillyTavern，我还需要什么？

SillyTavern 仅仅是一个前端界面，因此您需要访问上述列出的其中一个后端 API。

### 我如何能直接与开发者联系？

* Discord: cohee 或 rossascends
* Reddit: /u/RossAscends 或 /u/sillylossy
* [提交 GitHub 问题](https://github.com/SillyTavern/SillyTavern/issues)

### 我喜欢你的项目！我该如何贡献？

* 我们欢迎提交拉取请求！
* 我们也欢迎使用我们 GitHub 提供的模板提交有帮助且信息丰富的错误报告。
* 我们不接受针对项目本身的金钱捐赠。
* Cohee 不接受捐款。
* RossAscends 有个人的 [Patreon](https://www.patreon.com/RossAscends) 和 [Kofi](https://ko-fi.com/rossascends)
