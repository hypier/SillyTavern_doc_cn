# EmulatorJS

此扩展允许您直接从SillyTavern聊天中玩复古游戏主机游戏。

## 安装

**先决条件：**

- 最新版本的 SillyTavern。
- 从网上下载的 ROM 文件。您可以在 [任何地方](https://archive.org/details/ni-romsets) 找到它们。

**安装方法：**

1. 使用 SillyTavern 的扩展下载器进行安装。
2. 或使用此链接： `https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## 使用方法


- 打开 "EmulatorJS" 扩展菜单。
- 点击 "添加 ROM 文件"。ROM 文件保存在您的浏览器存储中，而不是存储在服务器上。
- 选择要添加的游戏文件。输入名称和核心（如果未自动检测）。如果核心需要 BIOS 文件，请也添加它。
- 点击列表中的 "播放" 按钮或通过魔杖菜单启动。
- 启动游戏后，您可以在模拟器界面中自定义控制和其他设置。
- 如果您需要休息，请使用保存/加载状态功能。

查看 EmulatorJS 文档以查看可用核心及其要求的列表：[系统](https://emulatorjs.org/docs/Systems.html)。

## 评论模式

借助像 GPT-4 Vision 这样的多模态模型，您的 AI 机器人可以观察您的游戏玩法并提供机智的角色内评论。

### 要求

1. 支持 [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility) 的浏览器。已在桌面 Chrome 上测试。Firefox 需要通过配置启用。Safari 不支持。
2. 建议使用图像内联模式。需要 OpenAI 或 OpenRouter API 密钥，选择“gpt-4-vision”作为模型；Google MakerSuite 的 Gemini Vision 模型；或 Anthropic Claude 3（推荐使用 Opus 模型）。
3. 如果禁用图像内联，请确保启用“图像字幕”扩展，然后选择“多模态”字幕源：
  - 具有访问“gpt-4-vision”模型的 OpenAI API。
  - 具有兼容多模态模型的 OpenRouter API。
  - 在 Koboldcpp 或 oobabooga TextGen WebUI 中本地托管的 Llava 模型。

### 如何启用评论

1. 确保您在 EmulatorJS 扩展设置中设置提供评论的间隔。该设置定义了角色使用您当前游戏玩法的快照查询评论的频率。值为 0 表示不提供评论。
2. 选择一个角色聊天并启动游戏。为了获得最佳性能，请确保 ROM 文件正确命名，以便 AI 能够获得更多背景上下文。
3. 像往常一样开始游戏。视觉模型将定期被查询，以根据它“看到”的最新截图撰写评论。

### 设置

1. 标题模板 - 用于描述游戏内截图的提示。支持 \{\{game\}\} 和 \{\{core\}\} 额外宏。
2. 评论模板 - 用于根据生成的标题撰写评论的提示。支持 \{\{game\}\}、\{\{core\}\}、\{\{caption\}\} 额外宏。在图像内联模式下，\{\{caption\}\} 被替换为 `see included image`。
3. 强制标题 - 将强制使用多模态标题，即使支持并启用了图像内联。

### 为什么我看不到任何评论？

如果出现以下情况，评论会暂时暂停（跳过间隔步骤）：

1. 模拟器已暂停（使用暂停按钮，而不是游戏内）。
2. 浏览器窗口失去焦点。
3. 用户输入区域不为空。这是为了让您可以安心输入回复。
4. 另一个回复生成正在进行中。
5. TTS 语音正在朗读。评论将被延迟（最多 20 秒）直到朗读完成，但不会被跳过。

其他常见问题：

1. 确保在启动游戏之前设置了评论间隔。
2. 确保您已设置多模态 API 密钥，并且 ST 服务器控制台没有错误。

仍然无法工作？请将您的浏览器调试控制台日志发送给我们（按 F12）。

## 版权

- EmulatorJS 引擎 (GPLv3): https://github.com/EmulatorJS/EmulatorJS