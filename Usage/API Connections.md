---
icon: repo-forked
---

# API 连接

SillyTavern 可以连接多种 LLM API。
以下是它们各自的优缺点和使用案例的描述。

## 本地 API

- 这些 LLM API 可以在您的个人电脑上运行。
- 它们是免费的，并且没有内容过滤器。
- 安装过程可能比较复杂（**SillyTavern 开发团队不提供支持**）。
- 需要单独从 [HuggingFace](https://huggingface.co/models?other=LLM) 下载 LLM 模型，每个模型可能在 5-50GB 之间。
- 大多数模型的性能不如云端 LLM API。

### KoboldAI

- 在您的个人电脑上运行，100% 私密，提供多种模型
- 提供对 AI 生成设置的最直接控制
- 需要大量 GPU VRAM（6-24GB，具体取决于 LLM 模型）
- 模型限制为 2k 上下文
- 不支持流式传输
- 受欢迎的 KoboldAI 版本：
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

### KoboldCpp

- 易于使用的 API，支持 CPU 卸载（对低 VRAM 用户有帮助）和流式处理
- 在 Windows 上通过单个 .exe 文件运行（在 MacOS 和 Linux 上必须从源代码编译）
- 支持 GGUF/GGML 模型
- 比 AutoGPTQ 和 Exllama/v2 等仅 GPU 加载器慢
- [GitHub](https://github.com/LostRuins/koboldcpp)

### Oobabooga TextGeneration WebUI

- 一体化 Gradio 用户界面，支持流式传输
- 对量化模型（AWQ, Exl2, GGML, GGUF, GPTQ）和 FP16 模型的广泛支持
- 提供一键安装程序
- 定期更新，有时可能会导致与 SillyTavern 的兼容性问题
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**正确连接 SillyTavern 到 Ooba 的新 OpenAI API 的方法**

1. 确保您使用的是 Oobabooga 的 TextGen 的最新更新（截至 2023 年 11 月 14 日）。
2. 编辑 CMD_FLAGS.txt 文件，并在其中添加 `--api` 标志。然后重启 Ooba 的服务器。
3. 连接 ST 到 <http://localhost:5000/>（默认情况下），并且不要勾选“Legacy API”选项。您可以从 Ooba 控制台提供的 URL 中移除 `/v1` 后缀。

*您可以通过 `--api-port 5001` 标志更改 API 托管端口，其中 5001 是您的自定义端口。*

### TabbyAPI

- 基于轻量级 [Exllamav2](https://github.com/turboderp/exllamav2) 的 API，支持流式传输
- 支持 Exl2、GPTQ 和 FP16 模型
- [官方扩展](https://github.com/theroyallab/ST-tabbyAPI-loader) 允许直接从 SillyTavern 加载/卸载模型
- 不推荐低 VRAM 用户使用（不支持 CPU 卸载）
- [GitHub](https://github.com/theroyallab/tabbyAPI)

## 云 LLM API

- 这些 LLM API 作为云服务运行，不需要您电脑上的资源
- 它们比大多数本地 LLM 更强大/智能
- 然而，它们都有不同程度的内容过滤，大多数需要付费

### Claude (由Anthropic提供)

- 推荐给希望其AI聊天具有创造性、独特写作风格的用户
- 提供4k、8k、100k上下文模型
- 所有API中最强的内容过滤器（截至2023年6月）
- 大多数模型访问受限
- 目前不接受新账户创建；处于候补名单中（2023年6月）
- [网站](https://www.anthropic.com/index/introducing-claude)

### DreamGen

- 无过滤的模型，专为可操控的人工智能角色扮演和故事创作而调优
- 每月免费积分，以及付费订阅
- 模型范围从 7B 到 70B
- [设置说明](/usage/api-connections/DreamGen/)

### Kobold Horde

- SillyTavern 可以直接访问此 API，无需额外设置
- 使用个别志愿者 (Horde Workers) 的 GPU 来处理您的聊天输入的响应
- 在生成等待时间、AI 设置和可用模型方面依赖于 Worker
- [Website](https://horde.koboldai.net)

### Mancer AI

- 提供无约束模型的服务，无需越狱。
- 使用“积分”来支付各种模型的代币。免费积分每天补充。
- 默认情况下不记录提示，但您可以启用此功能以获得代币的积分折扣。
- 使用类似于 `Oobabooga TextGeneration WebUI` 的 API，详细信息请参见 [Mancer 文档](https://mancer.tech/docs/clients/#sampling-parameters)。
- [网站](https://mancer.tech/)，[设置说明](https://docs.sillytavern.app/usage/api-connections/mancer/)

### NovelAI

- 无内容过滤
- 需要付费订阅
- [设置说明](https://docs.sillytavern.app/usage/api-connections/novelai/)

### OpenAI (ChatGPT)

- 易于设置和获取API密钥，提供4k-128k上下文模型
- 免费试用需要有效的电话号码
- 试用后，所有使用按月收费
- 角色扮演的写作风格可能会显得重复和可预测
- [设置说明](https://docs.sillytavern.app/usage/api-connections/openai/)

### OpenRouter

- WindowAI 浏览器扩展允许您使用自己的 API 密钥连接到上述云 LLM
- 使用 OpenRouter 支付以使用他们的 API 密钥
- 如果您不想在每个服务上创建单独的帐户，这将非常有用
- [WindowAI 网站](https://windowai.io) 和 [OpenRouter 网站](https://openrouter.ai)