# DreamGen

DreamGen 是一个用于 AI 驱动的角色扮演和故事创作的应用程序和 API。它们提供免费套餐，以及付费订阅，允许无限制地每月访问专门为可控 AI 角色扮演和故事创作而设计的高质量内部文本生成模型。创建一个账户以开始使用：<https://dreamgen.com/>.

（免费）积分在每个日历月的开始时重置。请查看 [定价](https://dreamgen.com/pricing) 以了解每个模型的积分成本，并查看 [使用情况](https://dreamgen.com/account/usage) 以查看您剩余的积分。

## 连接到 DreamGen

### 获取 API 密钥

访问 [DreamGen API 密钥](https://dreamgen.com/account/api-keys) 页面，并点击 "New API Key" 按钮。确保将 API 密钥复制到剪贴板中。

![创建新的 DreamGen API 密钥](/static/dreamgen/dreamgen_api_keys_new.jpg)
![复制 DreamGen API 密钥](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### 连接

1. 前往 SillyTavern 连接设置。
2. 选择 API: 文本完成
3. 选择 API 类型: DreamGen
4. 输入 API 密钥
5. （可选）选择一个模型

![连接到 DreamGen](/static/dreamgen/dreamgen_st_connection.jpg)

## 模型

DreamGen 提供 `opus-v1-sm`、`opus-v1-lg` 和 `opus-v1-xl`。模型越大，跟随指令和编写角色扮演和故事的能力就越强。

## 格式设置

DreamGen 模型期望特定的输入格式，详细信息请参见 [此处](https://dreamgen.com/docs/models/opus/v1)。

SillyTavern 提供了专为 DreamGen 设计的内置预设。请确保将这些设置作为您的基础。
这些设置尽量与 DreamGen 格式保持一致，但由于角色卡的格式不规则，可能并不总是完美。

1. 转到“高级格式”页面。
2. 在“上下文模板”下，根据模型选择 `DreamGen Role-Play V1 Llama3 / ChatML` (*).
3. **启用“指令模式”。**
4. 在“指令模式预设”下选择 `DreamGen Role-Play V1 Llama3 / ChatML`。

![DreamGen 上下文设置](/static/dreamgen/dreamgen_st_context_settings.jpg)
![DreamGen 指令设置](/static/dreamgen/dreamgen_st_instruct_settings.jpg)

(*) 什么时候使用 Llama 3，什么时候使用 ChatML？截至 2024/06/17，`opus-v1-sm` 是 `ChatML`，所有其他模型基于 `Llama3`。
运行本地模型时，模板将在模型的 HuggingFace 卡片中指明。

## 完成设置

DreamGen 支持：

-   "Temperature"、"Top P"、"Top K" 和 "Min P"
-   "Presence Penalty"、"Frequency Penalty" 和 "Repetition Penalty"（无范围）
-   "Min Length" -- 允许您强制模型生成至少 `min(min_length, max_tokens)` 个标记

好的起始值可能是：

-   Min P: 0.05
-   Temperature: 0.8
-   Repetition Penalty: 1.1

## 格式化提示

DreamGen 模型与常规的指令跟随模型（如 OpenAI 的 ChatGPT）有所不同。

这些模型经过微调，旨在根据提供的描述编写角色扮演或故事，通常包括情节描述、风格描述、角色、地点、背景等。模型还可以在角色扮演的过程中进行*引导*，让你成为导演，告诉角色应该做什么或情节应该如何展开。

一个格式良好的 **系统提示** 消息看起来应该是这样的：

```
You are an intelligent, skilled, versatile writer.

Your task is to write a story based on the information below.


## Plot description:

The librarian sets up a blind date between Lucifer and Mia. Lucifer immediately falls in love with Mia, but Mia needs more space and time to make up her mind.


## Style description:

The narrative is vivid and intensely sensual, with a strong emphasis on raw emotion conveyed from a first-person point of view. The language is explicit, evoking intense imagery and indulging in the erotic exploration of the characters' passionate encounters.


## Characters

### Lucifer

Lucifer, the red-skinned, horned demon, is the embodiment of fallen grace. Wrestling with his notorious heritage and a newfound desire for love, his complex nature ferments with vulnerability. His character oscillates between hedonism and self-reflection, hungering for acceptance by Mia and the librarian. Embracing his mortal love, he yearns for transformation, embodying the notion that even the damned may seek solace in love's redemption.

### Mia

Mia is a kind woman...
```

注意，**提示应该是故事的描述**，而不是关于如何编写故事的指令或指示。**避免使用以下短语：**

-   "将故事写成好像..."
-   "确保..."
-   等等。

请参阅更多 [示例](https://dreamgen.com/docs/models/opus/v1#task-story-writing)，了解情节、风格和角色描述应该是什么样的。

默认的 "DreamGen 角色扮演 V1" 模板将不同的部分替换如下：

-   `## Plot description:` 将由 {%{`{{scenario}}`}%} 和 {%{`{{wiBefore}}`}%} 组成。
-   `## Style description:` 不提供，您应该将其添加到系统提示的高级设置中，或者添加到角色卡的 {%{`{{scenario}}`}%} 末尾。此部分对影响叙述风格（第一人称、第二人称、第三人称）、时态（过去、现在）、细节和冗长程度等非常有用。
-   `## Characters:` 将有一个 {%{`{{char}}`}%} 角色，其描述由 {%{`{{description}}`}%} 和 {%{`{{personality}}`}%} 组成，以及一个 {%{`{{user}}`}%} 角色，其描述由 {%{`{{persona}}`}%} 组成。

### 消息示例和初始消息

DreamGen 模型对上下文非常敏感——它们在很大程度上会遵循之前对话回合中呈现的写作风格（和事实）。这使得消息示例和初始消息变得非常重要。

#### 格式化消息示例

{%{`{{mesExamples}}`}%} 被附加在系统提示的末尾。为了充分利用指令格式，请确保您的示例用 `<START>` 分隔符分开。例如：

{%{

```
<START>
{{user}}: (用户的回合)
{{char}}: (角色的回合)
<START>
{{user}}: (用户的回合)
{{char}}: (角色的回合)
```

}%}

### 示例

这里有几个为 DreamGen 适配的示例卡片，考虑到了独特的提示。这些卡片还利用了 {%{`{{mesExamples}}`}%}，如上所述。

#### Seraphina

这是对默认内置于 SillyTavern 的流行 Seraphina 卡片的编辑。

<div style="width:200px;">

[![Seraphina](/static/dreamgen/cards/Seraphina.png)](/static/dreamgen/cards/Seraphina.png)

</div>

#### Lara Lightland

这是 Deffcolony 的 Lara Lightland 卡片的编辑。

<div style="width:200px;">

[![Lara Lightland](/static/dreamgen/cards/LaraLightland.png)](/static/dreamgen/cards/LaraLightland.png)

</div>

## 常见问题解答

### 我应该使用什么采样器设置？

您可以从以下设置开始：

- Temperature: 1.0
- MinP: 0.05
- Presence Penalty: 0.1
- Frequency Penalty: 0.1

### 如何使回复更长或更短？

您有几个选项：

-   更改或添加系统提示或模型卡中的 `## Style description:`。您可以尝试添加类似于“句子通常较长，叙述详细描述环境”的内容。
-   更改完成设置中的 `Min Length`。
-   在指令模式下的高级格式设置中，添加类似于以下的 `Last Output Sequence`：

以下是一个可能有助于使模型以更详细方式响应的 `Last Output Sequence` 示例，使用 Llama 3 模板：

{%{

```
<|eot_id|>
<|start_header_id|>user<|end_header_id|>

Length: 400 words
Plot: {{char}} replies to {{user}} in detailed and elaborate way.<|eot_id|>
<|start_header_id|>writer character: {{char}}<|end_header_id|>


```

使用 ChatML 模板表达相同内容：

}%}


{%{

```

### 如何防止模型重复自己？

如果模型重复上下文中的内容，您可以尝试在完成设置中增加“重复惩罚”。或者您可以尝试重新措辞正在重复的上下文部分。
如果模型在一条消息中重复自己，您可以尝试增加“存在惩罚”或“频率惩罚”。

### 我该如何引导角色扮演？

如果您想指引角色做某事，或者将情节引导到特定方向，您可以使用 `user` 角色（即 `<!im_start|>user` 前缀）。

目前，这一功能尚未在 SillyTavern 中完美集成，但您可以使用上面描述的 `Last Output Sequence` 来插入 `user`（指令）回合。请参见 [示例](https://dreamgen.com/docs/models/opus/v1#prompt-instructions) 以了解指令应如何呈现。