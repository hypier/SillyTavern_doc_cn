---
order: 10
---

# OpenRouter

由于地理限制或候补名单无法访问 OpenAI / Claude API？使用 OpenRouter。

OpenRouter 的工作原理是让您使用他们拥有的密钥来访问像 GPT-4 和 Claude 2 这样的模型，所有服务都在一个共享的信用池中。

它提供免费试用（约 $1），之后为付费访问。没有订阅或每月账单 - 您只需为实际使用的部分付费。某些模型提供有限上下文大小的免费访问。

- [OpenRouter 定价详情](https://openrouter.ai/docs)
- 创建 OpenRouter 账户: [openrouter.ai](https://openrouter.ai/)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

从上到下（见上图）：

1. 选择“聊天完成”API
2. 选择 OpenRouter 源
3. 点击“授权”以使用 OAuth 流获取密钥。或者，可以在 [这里](https://openrouter.ai/keys) 生成 API 密钥并将其粘贴到框中。
4. 点击“连接”并选择模型
5. （可选）使用“测试消息”按钮验证您的连接

## WindowAI

[WindowAI](https://windowai.io) 是 OpenRouter 制作的浏览器扩展，可以控制您在任何启用的网站或网络应用上的 OpenRouter 连接。

您还可以在此处使用您自己的 Claude 和 OpenAI API 密钥。