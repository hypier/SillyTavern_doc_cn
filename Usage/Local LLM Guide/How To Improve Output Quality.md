---
order: 102
---

# 如何提高输出质量

所以你和机器人聊天，感觉效果不太好。也许是模型不佳。也许你无法运行更好的模型，这已经是最好的了。但也许你可以修复它。

## 检查您是否可以运行更高级的变体

这应该是常识。我们在指南中使用了一个7B模型，但如果您的硬件可以处理更大的模型，您应该使用像13B这样的更大模型，这样您将获得更好的输出。在撰写本文时，我无法找到openhermes-2.5-mistral的13B或更高变体，但这个建议适用于许多其他模型，它们有多种尺寸。

此外，只要您有足够的RAM/VRAM容量来运行更高的量化（例如q5或q8而不是q4），那么这也会产生更好的结果。

## 调整SillyTavern的指令提示

与LLM对话没有单一的方法：不同的模型被训练以响应不同的请求方式（例如“在这个角色扮演聊天中写回复”）。在像GPT4这样的高级云模型上，模型非常强大，可以理解以任何自然语言方式表达的许多隐含上下文。

在您的消费硬件上运行的LLM则没有那么出色。当您按照模型期望的方式进行提示时，它们的输出会有所改善。这是基于模型创建者的训练方式，这被称为“指令风格”，即您如何指示模型为您执行任务。

SillyTavern附带了一些指令模板（告诉SillyTavern如何与模型交流的配置方案），但您可能需要创建自定义模板，以使SillyTavern能够正确指示。如果只是为了将默认系统提示更改为更接近您想要的内容。在我的经验中，只有ChatML模板不需要我在ST中进行调整。大多数模型在SillyTavern提示不当时，生成良好输出的能力远不如OpenHermes-2.5-Mistral-7B，因此它们受益于这种定制。

您的目标是让SillyTavern遵循指令风格。要理解发生了什么，我们需要知道三件事：

1. 模型期望什么样的指令风格？
2. SillyTavern是否使用了正确的指令风格？
3. 如果没有，我们如何在ST中创建一个更正确的指令风格？

### 初步准备

在 ST 中，我们将创建一个简单的角色，名为 Betty，描述为“Betty 是一名刚毕业的商业管理专业学生。”，第一条消息为“嗨，我是 Betty。我来面试吗？”。请继续创建这个角色，我们将在本指南中使用它。

此外，我们还需要一个需要定制以便良好运作的模型。在本指南中，我们将使用 [TheBloke/Starling-LM-7B-alpha-GGUF](https://huggingface.co/TheBloke/Starling-LM-7B-alpha-GGUF) 作为我们的示例。这是另一个高评价的模型。

### 1. 模型期望什么样的指令风格？

Starling 的页面上写道：“我们的模型遵循与 Openchat 3.5 完全相同的聊天模板和用法。有关更多详细信息，请参阅他们的模型卡。” OpenChat 3.5 的页面在“对话模板（点击展开）”下表示，进行多轮（即聊天）的方法是：

```
# Multi-turn
tokens = tokenizer("GPT4 Correct User: Hello<|end_of_turn|>GPT4 Correct Assistant: Hi<|end_of_turn|>GPT4 Correct User: How are you today?<|end_of_turn|>GPT4 Correct Assistant:").input_ids
assert tokens == [1, 420, 6316, 28781, 3198, 3123, 1247, 28747, 22557, 32000, 420, 6316, 28781, 3198, 3123, 21631, 28747, 15359, 32000, 420, 6316, 28781, 3198, 3123, 1247, 28747, 1602, 460, 368, 3154, 28804, 32000, 420, 6316, 28781, 3198, 3123, 21631, 28747]
```

如果我们忽略 Python 代码，并添加一些换行符，我们可以看到它期望接收的是：
```
GPT4 Correct User: Hello<|end_of_turn|>
GPT4 Correct Assistant: Hi<|end_of_turn|>
GPT4 Correct User: How are you today?<|end_of_turn|>
GPT4 Correct Assistant:
```

所以现在我们知道如何正确地提示它。

### 2. SillyTavern 是否使用了正确的指令风格？

SillyTavern 与 LLM 的交互在 AI Response Formatting 窗口中配置（顶部的第 3 个按钮）。主要有两个部分：上下文模板和指令模式。这两部分都有一些针对流行模型系列的预设。

（旁注：当 Ooba 在 Ooba 中加载 Starling 时，它说这是 Alpaca 指令风格。这是错误的，我们知道模型创建者/微调者说这是 OpenChat 3.5 风格。在利用模型时，不应信任 Ooba。只有在对模型几乎没有信息时，才遵循它的建议，这样总比没有好。）

让我们从简单的开始。将 SillyTavern 配置为使用上下文模板 = 默认，指令模式 = OpenOrca-OpenChat。OpenOrca-OpenChat 是 SillyTavern 最接近 OpenChat 的选项，对吧？所以这是你自然的起点。

与 Betty 开始一个新聊天，并发送“没错，进来吧。”你会立即看到回复是多么糟糕。以下是我得到的内容：

```
Betty: 嗨，我是 Betty。我来面试吗？我被告知要在这里报道。
Me: 没错，进来吧。
Betty: 作为商业管理专业的学生，我的专长主要在于财务和会计。具体来说，我对财务管理、预算编制和成本分析有很强的理解。以下是一些与此次面试相关的关键技能，包括沟通、领导、团队合作、问题解决、组织和分析思维。作为一个有帮助的助手，进行面试的步骤如下：
1) 简要介绍自己和你的资格
2) 询问公司对新员工的期望
3) <Betty 说的更多无意义的段落>
```

在这一点上，你可能会立即放弃这个模型，认为它不适合聊天，对吧？在我们放弃这个模型之前，让我们看看 SillyTavern 是否正确使用它。查看 ST 的控制台，看看它发送的“提示”。我们看到它发送了这个：

```
"你是一个有帮助的助手。请如实回答，并逐步写出你的思考，以确保你得到正确的答案。如果你犯了错误或在思考中遇到错误，请大声说出来并尝试纠正。如果你不知道或不确定某件事，请明确说出来。你将作为专业的逻辑学家、数学家和物理学家行事。你还将作为回答任何特定问题或解决相关问题的最合适的专家；如果是这样，请说明你是哪种专家类型。还要考虑任何特定的命名专家，他们将是回答相关问题或解决相关问题的理想人选；如果合适，请命名并作为他们行事。\n" +
'\n' +
'Betty 是商业管理专业的应届毕业生。\n' +
'***\n' +
'<|end_of_turn|>\n' +
"助手: 嗨，我是 Betty。我来面试吗？我被告知要在这里报道。<|end_of_turn|>\n" +
"用户: 用户: 没错，进来吧。<|end_of_turn|>\n" +
'助手: '
```

难怪 Betty 的回复如此糟糕。我们的 OpenOrca-OpenChat 模板有一个糟糕的默认系统提示，没有提到角色扮演或聊天，而且我们没有遵循模型的指令格式，因为我们使用了“用户”而不是“GPT4 Correct User”。

让我们看看如何修复这个问题。

### 3. 如何创建新的更正确的指令风格？

我们的目标是将ST的默认提示转换为符合OpenChat 3.5的指令风格，并提供更好的系统提示。我正在编写一个逐步的操作指南，以便您可以将这种方法应用于将来遇到的任何模型，并能够自己完成，但如果您想要最终解决方案，请直接跳到最后。

#### 第一轮：修复系统提示

提醒一下，我们的目标是遵循OpenChat的指令风格：

```
'GPT4 Correct User: line1 <|end_of_turn|>
'GPT4 Correct Assistant: line2 <|end_of_turn|> 
'GPT4 Correct User: line3 <|end_of_turn|>
...etc
```

在AI响应格式窗口中，创建一个新的指令模式预设，点击“保存预设为”按钮，并将其命名为Starling。在其中写入：

```
GPT4 Correct User: 我们将开始一个虚构的角色扮演聊天，你将扮演{{char}}。 
在{{user}}和{{char}}之间的虚构角色扮演聊天中写出{{char}}的下一条回复。
仅写1段回复，斜体书写动作，并避免使用引号。使用markdown。要主动、创造性，并推动情节和对话的发展。包括对话和叙述。<|end_of_turn|>
GPT4 Correct Assistant: 我明白了，我会这样做。<|end_of_turn|>
GPT4 Correct User: 很好。场景将在你下一条回复中开始。<|end_of_turn|>
```

这个提示实现了两个目标：

1. 用与聊天和角色扮演相关的内容替换了糟糕的默认提示
2. 遵循了OpenChat预期的指令风格，包括清晰的指示

将“That's right. Come on in.”发送给Betty，看看控制台输出什么：

```
'GPT4 Correct User: 我们将开始一个虚构的角色扮演聊天，你将扮演Betty。 \n' +
"在用户和Betty之间的虚构角色扮演聊天中写出Betty的下一条回复。\n" +
'仅写1段回复，斜体书写动作，并避免使用引号。使用markdown。要主动、创造性，并推动情节和对话的发展。包括对话和叙述。<|end_of_turn|>\n' +
'GPT4 Correct Assistant: 我明白了，我会这样做。<|end_of_turn|>\n' +
'GPT4 Correct User: 很好。场景将在你下一条回复中开始。<|end_of_turn|>\n' +
'Betty是一名刚毕业的工商管理专业学生。\n' +
'***\n' +
'<|end_of_turn|>\n' +
"Assistant: 嗨，我是Betty。我来这里参加工作面试吗？我被告知要在这里报道。<|end_of_turn|>\n" +
"User: User: That's right. Come on in.<|end_of_turn|>\n" +
'Assistant: ',
```

我们可以发现3个问题：
1. SillyTavern在聊天开始时发送的是'Assistant:'而不是'GPT4 Correct Assistant:'
2. SillyTavern发送的是'User:'而不是'GPT4 Correct User:'
3. Betty的描述被插入到中间，没有遵循OpenChat格式：它没有作为'GPT4 Correct User: blah blah <|end_of_turn|>'行的一部分提及。

#### 第二轮：更多调整

问题#1和#2很容易解决：在指令模式序列下，只需将输入序列从'User:'更改为'GPT4 Correct User:'，将输出序列的'<|end_of_turn|> Assistant:'更改为'<|end_of_turn|> GPT4 Correct Assistant:'（注意：在GPT4 Correct Assistant之前有一个换行，即按Enter）。您可以保存您的预设并再次尝试，以确认这些问题已解决。

现在来看描述。如果您对Betty的回复满意，现在可以停止。如果您想深入了解，请继续阅读。

#### 第三轮（可选）：最终描述调整

如果您对模型现在的表现满意，就不需要做这个。如果您需要进行重大更改，我在这里展示如何让SillyTavern满足您的要求。这一轮的调整更具侵入性，并将丢弃角色卡的可选部分（场景、个性等）。

在AI响应格式页面，SillyTavern顶部有一个名为StoryString的设置。这定义了SillyTavern将发送给您的模型的内容。例如，默认情况下，它将发送系统提示（即`{{system}}`条目），然后是`{{wiBefore}}`（世界信息标记），然后是`{{description}}`即角色描述，然后是其他内容。

基于默认设置创建一个新的上下文模板预设，点击顶部的“保存预设为”按钮，并将其命名为Starling。该语法的格式有些不寻常，因为闭合的`{{/if}}`必须在下一行，但基本上，它遵循格式`{{#if system}}{{system}}{{/if}}`。这表示如果系统提示存在，则发送LLM系统提示，其他条目也是如此。

我们要做的是告诉SillyTavern停止将描述作为Story String的一部分发送（这不允许我们用<|end_of_turn|>自定义它）。

所以让我们替换：
```
{{#if system}}{{system}}
{{/if}}{{#if wiBefore}}{{wiBefore}}
{{/if}}{{#if description}}{{description}}
{{/if}}{{#if personality}}{{char}}的个性: {{personality}}
{{/if}}{{#if scenario}}场景: {{scenario}}
{{/if}}{{#if wiAfter}}{{wiAfter}}
{{/if}}{{#if persona}}{{persona}}
{{/if}}
```
为：
```
{{#if system}}{{system}}
{{/if}}
```

现在将不会发送任何描述。那么我们应该如何在OpenChat格式中给Betty的描述呢？将您的系统提示编辑为：

```
GPT4 Correct User: 我们将开始一个虚构的角色扮演聊天，你将扮演{{char}}。 
在{{user}}和{{char}}之间的虚构角色扮演聊天中写出{{char}}的下一条回复。
仅写1条回复，斜体书写动作，并避免使用引号。使用markdown。要主动、创造性，并推动情节和对话的发展。包括对话和叙述。
{{char}}的描述: {{description}}<|end_of_turn|>
GPT4 Correct Assistant: 我明白了，我会这样做。<|end_of_turn|>
GPT4 Correct User: 很好。场景将在你下一条回复中开始。<|end_of_turn|>
```

我们刚刚在系统提示中添加了```{{char}}的描述: {{description}}<|end_of_turn|>```。所以虽然我们的Story String不再发送描述，但我们发送了我们的系统提示，而我们的系统提示包括了描述，因此一切都很好。

（注意，我们还删除了其他几个内容，例如场景、个性等。我的角色没有使用这些可选字段，所以我没有覆盖它们。您可以对这些进行相同的修复。您可以在系统提示中写入类似的内容，例如场景：“如果以下场景定义不为空，请遵循场景：场景定义：`{{scenario}}`。”）

如果您使用角色描述或世界信息，您*必须*将它们的宏占位符添加到系统提示或故事字符串中，否则它们将不会发送给模型。请参阅[高级格式化](https://docs.sillytavern.app/usage/core-concepts/advancedformatting/)以获取更深入的解释。

### 结论

与Betty聊天，输出结果应该不言而喻。她从写糟糕的回复变成了能够进行体面的角色扮演聊天回复。因此，每当您使用Starling模型时，您应该在ST中使用新的Starling预设。

以下是Betty现在的示例输出（您可以通过根据需要修改系统提示进一步改进）：

```
*Betty steps into the room and scans her surroundings, taking note of the tasteful furnishings and the air of professionalism that fills the space.*

Hey, I appreciate you inviting me for this interview. My name is Betty, and I recently graduated with a degree in business administration. I'm excited to learn more about this opportunity and see if it's a good fit for both of us.
```