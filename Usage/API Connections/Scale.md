# Scale

Scale 是一种通过部署的“应用程序”轻松访问 GPT-4 和其他 LLM 的方式，这些应用程序充当 API 端点。

目前，Scale 不支持令牌流和通过 SillyTavern 的 UI 配置参数如温度。

**Scale API 不是免费的，但如果您链接信用卡，可以提供 $5 的试用。**

## 快速入门

- 在 <https://spellbook.scale.com> 创建一个 Scale Spellbook 账户（如果您的国家不支持，请使用 VPN）
- 创建一个任意名称和描述的“应用”
- 创建一个“变体”，设置参数（系统提示、模型、温度、响应令牌限制等）
- 选择一个合适的语言模型进行部署（推荐使用 GPT-4）
- 用以下内容替换提示的“用户”部分：

>Complete the next response in this fictional roleplay chat.
>
>\{\{ input \}\}

- 配置模型参数。
  - **模型:** GPT-4
  - **温度:** ~0.6 - 0.9
  - **最大令牌:** 400 - 600（根据消息长度偏好）
- 点击“保存新变体”
- 前往您的新变体并点击部署
- 这将为您的机器人创建一个 API 密钥和 URL
- 导航到 SillyTavern，选择“聊天完成”API 和 Scale 源
- 将 API 密钥和 URL 粘贴到相应字段中并点击“连接”

## 贡献

实现和文档灵感来源于 [khanon](https://github.com/khanonners) 的工作：<https://github.com/khanonners/TavernAIScale>