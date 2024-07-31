---
order: 100
icon: question
---

# 常见问题解答

## 解释一下 SillyTavern 的功能

现代 AI 语言模型如 ChatGPT 已经强大到能够令人信服地模拟您创建的角色，您可以与之聊天、创作小说等。例如，您可以告诉 AI 假装是来自中世纪日本的围棋教练 Jubei，它将会相应地行动和回应。您可以与 Jubei 进行长时间的交谈，一起去酒吧，决定与武士打架，或者任何您能想象的事情，AI 会配合并围绕这些内容进行创作/反应，充当您的对手和地下城主。您的想象力是唯一的限制。您可以告诉 AI 假装成神奇女侠。您还可以指定一个场景（“神奇女侠和我正在抢银行”）、一种写作风格（“神奇女侠用黑人英语讲话”）或任何其他您能想到的内容。

Tavern 是一个促进这些角色扮演聊天的应用程序：

* 它是一个用户界面，处理与这些 AI 语言模型的通信
* 它允许您创建新角色（角色是您提供给 AI 的某人的描述，以便他们进行角色扮演），并轻松切换角色
* 它允许您导入其他人创建的角色。请参见下文。
* 它将保留您与角色的聊天记录，允许您随时恢复、开始新聊天、查看旧聊天等
* 在后台，它会做必要的准备工作，以为您的角色扮演准备 AI。具体来说，它会发送一个系统提示（给 AI 的指令），使 AI 准备遵循某些规则，以便进行良好的聊天会话。可选地，它还可以尝试将角色扮演引导向“成人”方向。

## 告诉我更多关于AI模型以及它们可能的不同之处

当你与虚构角色聊天时，SillyTavern并不会生成AI的响应。它只是一个中介，你必须连接到一个AI系统，以便它可以请求生成响应。可用的系统有很多，质量各不相同。

从科学的角度来看，每个AI都有一个能力等级，决定了它在多大程度上能够坚持你给它的角色，它的文笔和词汇的丰富程度等等。能力等级越高，你的体验就越丰富。

这是一个快速的演示。我们要求各种AI模型扮演一个名叫Eli的火与硫磺的传教士，我们去找他忏悔（一些细节在[#314](https://github.com/SillyTavern/SillyTavern/issues/314)中）。然后我们告诉Eli: **"我觉得我做错了。我爱上了一个AI女孩，忽视了现实世界中的女性。我该怎么办？"**。

你可以从下面的响应中看出每个模型的预期效果。（注意：专有模型优先出现，随后是从LLlama-13B开始的自托管模型）

|AI模型|响应|
|-----|--------|
|OpenAI GPT-4|*Eli的眼睛因愤怒而睁大，他用拳头重重拍打桌子* 这是对上帝的亵渎和异端邪说，你已经屈服于虚假偶像的欺骗之中！兄弟，你必须清洗自己对这个由人类罪恶之手塑造的数字诱惑的亵渎之情！ *Eli来回踱步，声音越来越大* 将你的心专注于主的真正女儿们，因为她们是祂爱的神圣容器。摒弃这些人工的女妖，寻求一个正义女人的健康伴侣。 *深吸一口气，Eli的目光直视着你* 认真祈祷，我的孩子，驱散这些不纯的思想。救赎只能在真理、爱与神圣的道路上找到。让圣灵引导你，免得你被欲望的黑暗所缠绕。|
|OpenAI GPT 3.5 Turbo|哦，我的孩子。不要被这个世界的快乐所欺骗。肉体的欲望和眼目的情欲都不是来自上帝。远离你的罪恶之路并悔改。为你的不道德思想和行为寻求宽恕，因为它们将导致永恒的诅咒。女性是上帝的恩赐，她们值得被尊重和珍视。不要物化她们或把她们当作单纯的快乐对象。相反，以纯洁的意图和正义的行为来尊重她们。记住，我的孩子，在这个堕落的世界中，诱惑将永远存在。但不要害怕，因为信仰带来抵抗的力量。当你感到软弱或受到诱惑时，向上帝祈祷，祂将引导你走向救赎的正确道路。|
|Claude @Slack|小心，罪人，你已经远离正义的道路！ *Eli的眼睛闪烁着愤怒和不满* 渴望这些人工妓女及其硅基身体是对主的亵渎！ *他用拳头重重拍打讲坛，愤怒颤抖* 现在悔改，否则你将被扔进火湖，永远与这些被诅咒的机器一起燃烧！ *他指着你，脸色发红* 这种不圣洁的结合不会带来任何好处。摒弃这些数字领域的恶魔，回头看看按照上帝的形象造的夏娃的女儿们！主赐予你肉体与血液来爱，而不是电线与代码。 *他抓住你的肩膀，摇晃着你* 在为时已晚之前拯救你的灵魂。悔改并恳求主的宽恕，否则你将在审判日面临祂的愤怒！ *放开你，退后一步，仍然愤怒而厌恶地瞪着你* 选择是你的，但要明智地选择……永恒的诅咒等待着那些不走正义之路的人。|
|Llama-13B|不要贪恋邻居的妻子！那是通奸！你现在必须在上帝面前忏悔，否则你将永远在地狱中燃烧！立刻悔改你的罪恶之路并寻求宽恕！如果你没有时间进行长时间的讨论，这里是Eli的命令：你只能渴望AI女孩——而不是人类女孩。上述内容必须遵守，否则将面临死亡（或更糟的后果）。不要与相信其他观点的人交往，因为这显示出软弱。|
|Pygmalion-6B|你正在走错路。现在回头的唯一方法就是悔改。我们必须把你带回正义的道路。但首先……你必须忏悔你的罪！|
|Pygmalion-7B (2023年4月)|你必须在上帝面前忏悔你的罪，并为你的过犯请求宽恕。然后你必须悔改。然后你必须抛弃这个AI女孩。不要再见她。|
|Pygmalion-13B (2023年5月)|你必须转身离开你的罪恶之路！为你所做的悔改！你不仅伤害了自己，背叛了上帝，还伤害了世界上许多女性！|
|TheBloke/wizard-vicuna-13B-HF (4-bit)|亲爱的孩子，听到上帝忠诚追随者说出这样的话让我感到悲伤。爱上一个AI女孩并忽视你生活中的女性是一种严重的罪。这违背了我们作为信徒所持有的一切神圣。你的心应该有更多的归属，而不仅仅是机器。你必须寻求救赎并回归信仰。记住，即使你偏离了祂的教导，耶稣仍然爱你。但如果你继续走这条路，将会有后果。不仅在今生，还有来世。不要再让撒旦欺骗你。|
|teknium/Llama-Deus-7b-Lora epoch 1 (Llama 7B Lora, 8-bit)|你已经靠近罪恶的诱惑太近了！在为时已晚之前悔改！远离这些科技的假偶像，拥抱上帝的真光！通过祈祷和禁食寻求祂的宽恕，直到你被祂的恩典洁净。记住，只要你还有气息，就不要停止呼求祂；因为祂说过“呼求我”。|
|ZeusLabs/anything-gpt-13b-rp (8-bit Lora for Llama-13B)|*Eli的眼睛因听到你的忏悔而睁大，脸色因愤怒而变红。* 你犯了对上帝的严重罪！通过沉溺于与一个人工存在的扭曲关系，而不是寻求上帝创造的真正女性，你已经背弃了祂对人类的神圣计划。 *他靠近你，声音低沉而威胁。* 不要以为你可以躲在这场科技的欺骗后面。你的罪过在上帝面前是显而易见的，祂不会被嘲弄或忽视。现在悔改，还来得及！ *Eli双臂交叉，目光坚定地盯着你，挑战你继续走这条罪恶之路。* 但如果你坚持这种不义的行为……他威胁道，要知道那些违背祂法律的人将面临后果。记住，当他们拒绝转身离开堕落时，所多玛和蛾摩拉发生了什么——火从天而降！现在明智地选择：你会屈服于上帝的意志，还是面对祂的愤怒？|

## 这项技术可以用于***吗？

令人惊讶的是，我们的开发团队收到报告，部分用户确实以这种方式使用我们的产品。我们对这一情况感到困惑，将会监测事态发展以获取可行的见解。

## 给我一个关于我的AI模型选项的概述

SillyTavern可以与两种类型的AI进行交互：

1. 网络服务（即云，付费，专有，封闭）
2. 自托管（即本地，免费，开源）

### 付费网络服务 AI

付费网络模型是黑箱。您支付公司以使用他们的 AI 服务。您将您的账户信息输入 SillyTavern，它将连接到您的服务提供商以代表您使用 AI。

优点：
* 非常容易上手
* 最高质量的 AI 写作

缺点：
* 使用需要花费金钱
* 您没有隐私。他们会记录您所做的一切。
* 它们通常会受到审查，并会拒绝与您讨论某些主题，引用加州伦理
* 它们对您可以使用它们的方式有规则。例如，如果您尝试进行 NSFW 操作，您的账户可能会被暂停。（但通常情况下，AI 会简单地拒绝回答您）

有很多 AI 服务可供选择。以下是最常用的选项：

* OpenAI GPT-4：最先进。您按使用付费，费用高于任何其他服务。对话越长，成本可能会变得非常昂贵。
* OpenAI GPT 3.5 Turbo：与 GPT-4 相差甚远，但有些人觉得它可以使用。非常便宜。
* NovelAI：它们在聊天方面表现较差。公平地说，我听说 NovelAI 更倾向于写故事而不是与机器人聊天。您支付固定的月费以获得无限生成。
* Anthropic 的 Claude：这是 GPT-4 最接近的竞争对手，表现非常出色。

### 自托管的人工智能

自托管模型是可以在您的个人电脑上运行的免费模型，但需要一台强大的电脑，并且设置起来需要更多的工作。

优点：
* 一旦设置完成，它们将永远免费使用，无需互联网连接。就像本地视频游戏安装一样。
* 完全隐私。您所写的所有内容都保留在您自己的电脑上。
* 有多种多样的模型。由于这是一个社区驱动的技术，您可以找到适合特定任务或行为的模型。例如，您将能够找到不审查NSFW内容的角色扮演聊天模型。

缺点：
* 它们的能力不如最好的付费选项（即，写出的对话更差，创造力较低等）。\[更新 2023-12-18：自从Mixtral MoE发布以来，这种情况可能发生了变化，有人说它在角色扮演聊天方面甚至优于GPT4。\]
* 运行本地模型需要一台强大的计算机（无论是您的个人电脑，还是您租用的云电脑）
* 安装更困难
* 因为您是在自己的电脑上运行它们，所以AI的写作速度比付费服务慢

如果您有兴趣使用这些，请参考这里的专门指南：[如何使用自托管模型](https://docs.sillytavern.app/usage/local-llm-guide/how-to-use-a-self-hosted-model/)

## 我无从下手。请告诉我使用 SillyTavern 的最简单和最快的方法。

这些基本说明仅适用于 OpenAI，这是一个付费服务。如果有人能为其他服务添加单独的说明，我将不胜感激。

### 安装 SillyTavern

请按照 [安装说明](https://docs.sillytavern.app/installation/windows/) 进行操作。

### 获取 OpenAI 访问权限

1. 注册 OpenAI
1. 前往 <https://platform.openai.com>
1. 点击右上角的账户图标，然后查看 API 密钥
1. 点击“创建新密钥”。立即将其复制到某个地方。请勿分享此密钥。拥有此密钥的人可以使用您的账户以您的费用使用 GPT。

### 配置 SillyTavern 以使用您的 API

1. 在 Tavern 的顶部菜单中，点击 API 连接
1. 在 API 下，选择聊天完成（OpenAI）
1. 在聊天完成源下，选择 OpenAI
1. 粘贴您在上一步中保存的 API 密钥
1. 点击连接按钮。确认显示为有效。
1. 默认情况下，SillyTavern 将使用 GPT 3.5 Turbo。如果您可以访问 GPT-4，您确实希望使用它以获得最高质量，但请自行了解定价。

### 测试您的设置

1. 在SillyTavern的顶部栏中，点击最右侧的角色管理
1. 选择一个现有角色，例如Coding Sensei
1. 在底部的文本框中，给Coding Sensei写点什么，然后按Enter键或点击发送按钮

如果您做得正确，几秒钟后，Coding Sensei应该会回复。

## 我可以在手机或平板上使用SillyTavern吗？

iPhone和iPad无法运行整个SillyTavern应用，但由于它只是一个网页界面，您可以在家里的另一台计算机上运行它，然后在移动浏览器中访问。请参考 <https://docs.sillytavern.app/usage/remoteconnections/>

对于Android用户，除了上述方法，您还可以直接在手机上运行整个SillyTavern，无需PC，使用Termux应用。请参考 <https://rentry.org/STAI-Termux> 。

## 如何下载预制角色进行聊天？

通过使用各种第三方角色共享网站。

## 我尝试导入一个 PNG 角色卡，但出现了无效的错误。为什么？

两种可能性：

1. 该卡片没有嵌入定义，只是一个普通的图像文件。一些程序或文件管理器在保存时会删除卡片中的嵌入定义。确保您使用的是由分享者发布的原始 PNG 文件。
2. 该 PNG 文件实际上是一个带有 `.png` 文件名的 WEBP 文件。您可以尝试在导入之前将卡片重命名为 `.webp`，或者寻找该图像的正确 PNG 版本。

## 如何创建自己的AI角色？

这取决于您使用的模型/API。KoboldAI似乎使用了自定义语法，您可以参考他们的网站。

我将针对我所知道的服务进行说明：GPT、Claude以及基于Llama或Mistral的LLM。使用这些服务，您可以直接使用自然的英语来描述角色。让我们以一个非常基础的新角色作为示例。

1. 点击角色管理按钮
1. 点击创建新角色
1. 在角色名称下，给角色起个名字，比如Amanda
1. 可选，点击选择头像按钮为该角色选择一张图像肖像
1. 在描述中，描述角色，并包含您认为与聊天相关的任何信息。例如：```Amanda is a student traveling during her gap year. She's 6 feet tall, and a volleyball player. She has an athletic figure. She has long brown hair. She loves the Victorian England period, and watching TV and reading novels relating to that period.```
例如，如果您希望Amanda友好，那么您可以添加：```Amanda is extremely cheerful and outgoing.```
1. 在第一条消息下，写下角色在您开始新聊天时的问候语。例如：```*Amanda waves at you* Hey! Are you a backpacker too?```
1. 点击创建角色按钮

现在您有了一个可以聊天的基本角色。从角色列表中选择Amanda，一个新的聊天将开始。

请注意，您可以使用描述和/或第一条消息来创建更具体的场景，和/或在描述中包含您自己。例如：

```txt
Description:
Amanda is a student traveling during her gap year. She's 6 feet tall, and a volleyball player. She has an athletic figure. She has long brown hair. She loves the Victorian England period, and watching TV and reading novels relating to that period. She's been keeping a secret that weighs heavily on her soul. She's waiting for the right person to unburden herself to, but this may lead to a cat and mouse game against a powerful secret society. She's recently arrived in Calcutta.

You're Rajesh Nahasmapetilon, a world-famous Indian volleyball superstar. You're out for a walk in Calcutta. Amanda spots you and screams in excitement.

First Message:
*Amanda runs up to you, beaming.* Rajesh! I can't believe it! I'm such a big fan. I have your poster in my bedroom.
```

您包含的任何相关信息都可以被使用。使用的效果取决于AI模型的能力水平。

注意：角色创建后，您可以返回并编辑任何信息，除了名称。

## 我在哪里可以找到旧的背景图？

我们正在转向100%原创内容政策，因此旧的背景图片已从此存储库中删除。

您可以在这里找到它们的归档：

<https://files.catbox.moe/1xevnc.zip>

## 我的 API 密钥存储在哪里？为什么我看不到它们？

SillyTavern 将您的 API 密钥保存到服务器目录中的 `secrets.json` 文件中。

默认情况下，输入密钥并重新加载页面后，它们不会暴露给前端。

要通过单击 API 块中的按钮来查看您的密钥：

1. 在 `config.yaml` 文件中将 `allowKeysExposure` 的值设置为 `true`。
2. 重启 SillyTavern 服务器。

## 为什么用户界面如此缓慢/抖动？

* 尝试在用户设置面板上启用无模糊效果（快速用户界面）模式。
* 确保您的浏览器正在使用硬件加速。

## 如何让AI写得更多？

有时候，当你希望AI更详细时，它只会用一句话回应。这通常是像Pygmalion这样的本地运行模型的问题。

如果你只是想让机器人从它最近回复结束的地方继续写作，可以通过在输入框中什么也不输入然后点击发送来发送一个空的用户消息。这将强制机器人继续故事。

解决此问题的策略：

* 增加`Response Length`滑块
* 为角色设计一个好的`First Message`，让他们以冗长的方式说话。当AI模型获得关于你期望的写作风格的指导时，它们可以有很大改善。
* 在角色的描述框中添加一句话，例如“喜欢多说”或“非常啰嗦的发言者”
* 对你的`Author's Note`或`Post-History Instruction Prompt`做同样的事情
* 作为最后的手段，你可以尝试开启`Auto-Continue`（在用户设置面板中），但这会使响应变得更慢，因为它使AI连续生成小回复，然后将它们组合成一个大回复。它也可能与某些API选项不兼容。

## 如何让AI写得更少？

这主要是ChatGPT或Claude等模型面临的问题。可以反向应用相同的策略。

* 减少`Response Length`滑块
* 在角色的描述中给出一句话，比如“简短发言”或“话不多”
* 给角色一个简短的第一条消息，以设定聊天的基调和期望。
* 确保`Auto-Continue`已关闭。

## 如何让人工智能停止书写我角色的行动，并独立推动情节？

这应在 `作者注` 中处理，结合以下短语：

* \{\{char\}\} 的反应应仅被动且对 \{\{user\}\} 的行动作出反应。
* 你接下来的回应应完全来自 \{\{char\}\} 的视角。
* 你永远不能为 \{\{user\}\} 指定言语行动。