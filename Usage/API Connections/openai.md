---
order: 20
---

# 聊天完成

聊天完成 API 包括 OpenAI、Claude 和 PaLM。  
WindowAI 和 OpenRouter 也允许连接这些 API。

## OpenAI

### API 密钥

**获取方法：**

1. 访问 [OpenAI](https://platform.openai.com/) 并登录。
2. 使用 "[查看 API 密钥](https://platform.openai.com/account/api-keys)" 选项创建新的 API 密钥。

**重要！**

*丢失的 API 密钥无法恢复！请确保妥善保管！*

## Claude

如果您可以访问Anthropic的Claude API：

- 选择“Claude”作为“聊天完成源”。
- 输入您的API密钥。
- 点击连接。

## 代理

!!!warning
请注意，我们不提供对您可能遇到的任何问题的支持！
我们不保证与每个可能的 API 端点的兼容性！
!!!

!!!
如果您打算使用此代理功能来使用本地端点，例如 TabbyAPI、Oobabooga、Aphrodite 或类似的服务，您可能想要查看 [这些的内置兼容性](https://docs.sillytavern.app/usage/api-connections/)。此代理功能主要用于与其他提供 OpenAI 兼容 API 聊天完成端点的服务和程序一起使用。

大多数文本完成 API 支持比 OpenAI 标准允许的更大的自定义选项。这些更大的自定义选项，例如 Min-P 采样器，可能值得 SillyTavern 用户查看，因为它们可以大大提高生成的质量。
!!!

可以为 OpenAI 的后端配置代理/替代端点。此自定义端点可以连接到支持通用 OpenAI API 架构的替代聊天完成 API。

实现此 API 的后端示例包括：

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://litellm.ai/)
* [LocalAI](https://localai.io/)

此功能通过以下方式访问：

- 切换到“聊天完成”API 类型。
- 选择“OpenAI”作为“聊天完成来源”。
- 将 API 密钥等详细信息留空。
- 打开“AI 响应配置”选项卡并向下滚动到“OpenAI / Claude 反向代理”部分。

在这里，您可以输入代理/自定义端点，并在需要时在“代理密码”下可选地输入 API 密钥。
例如，TabbyAPI 会为您提供一个必须使用的 API 密钥。

返回到“AI 连接”选项卡，您可以找到两个可选复选框，标记为：

- 跳过 API 状态检查。
- 显示“外部”模型（由 API 提供）。

选中“跳过 API 状态检查”会告诉 SillyTavern 停止提醒您关于非功能性 API 端点的警告。如果您的 API 端点正常工作，但 SillyTavern 仍然不断警告您，请选中此项。

> **提示：** 如果它不起作用，请尝试在端点 URL 的末尾添加 `/v1`！

选中“显示‘外部’模型（由 API 提供）”将显示您自定义 API 端点报告的可用外部模型（向下滚动经过 OpenAI 的模型）。这使您可以直接从 SillyTavern 选择不同的 API 模型，而无需进入自定义应用程序并更改模型。

**此功能不是自定义 API 端点正常工作的必要条件**，并且可能并非在每个后端都可用。