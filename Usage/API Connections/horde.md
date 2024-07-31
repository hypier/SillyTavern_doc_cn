# KoboldAI Horde

## 免责声明

- KoboldAI Horde 是一个完全由志愿者运行的众包分布式 GPU 集群。
- 默认情况下，您的输入会匿名发送，运行 Horde Worker 的人无法看到响应。
- 然而，由于这是一个开源程序，恶意工作者可能会修改代码来：
  - 记录您的活动（输入提示、AI 响应）。
  - 生成不良或冒犯性的响应。

!!!警告
使用 Horde 时**绝不要发送**任何个人信息，例如姓名、电子邮件地址等。
!!!

启用“仅限受信任工作者”复选框将限制可用工作者的选择，仅限于那些在 Horde 上托管了一段时间并被普遍认为是可信的工作者。但他们仍然可能会看到提示，例如通过使用未记录的软件进行托管。

为了帮助减少这个问题，SillyTavern 内置了以下功能：

- 当 Horde Worker 生成聊天响应时，SillyTavern 会记录工作者的 ID 及其使用的模型。
- 通过将鼠标光标悬停在聊天项目上，您可以查看此信息（见下图）。
- 如果您认为收到了恶意响应，可以将此信息传递给 Horde 管理员，以便在 [Kobold Discord](https://koboldai.org/discord) 上进行审核和可能的纪律处分。

![Horde Worker Info Popup](/static/horde-worker.png)

## 设置

- SillyTavern 可以开箱即用地连接到 Horde，无需额外设置。
- 在 ST API 面板的 API 下拉选择器中选择 'KoboldAI Horde'。
- 在面板底部的模型选择器中选择一个或多个模型（角色的 'AI brains'）。
- 选择一个角色并开始聊天。

  ![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
默认情况下，您的 SillyTavern 实例连接到 Horde 的低优先级 'guest account'。
这意味着您可能需要等待很长时间才能收到回复。
要减少等待时间，请遵循下面的提示。
 !!!

## 提示

- [在Horde网站注册一个账户](https://horde.koboldai.net/register)，然后将你的Horde密钥添加到SillyTavern Horde API Key框中。
- [设置一个Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme)来为他人提供你的GPU。
  - 让他人使用你的GPU可以赚取['Kudos'，一种仅限Horde的货币](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos)。
  - 你的账户拥有的kudos越多，你将从其他Horde Workers那里获得聊天响应的速度就越快。
  - Kudos还可以用于在[Stable Horde](https://stablehorde.net)上创建AI图像。
    - SillyTavern支持开箱即用的Stable Horde图像生成。
- 如果你的GPU不够强大无法运行AI，或者你没有电脑，你仍然可以通过[参与Horde社区以各种方式赚取Kudos](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos)。