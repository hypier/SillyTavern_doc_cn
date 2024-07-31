# 高级格式设置

本节提供的设置允许对提示构建策略进行更精细的控制。

### 上下文模板

**这里的大多数设置不适用于聊天完成 API，因为它们由提示管理系统控制。**

通常，AI 模型需要您以某种特定方式向它们提供角色数据。SillyTavern 包含不同模型的预制转换规则列表，但您可以根据需要自定义它们。

#### 故事字符串

此字段是预聊天角色数据的模板（内部称为故事字符串）。
这是为文本完成和指导模型格式化角色卡的主要方式。

该模板支持 Handlebars 语法以及任何自定义文本注入或格式化。请参见语言参考： https://handlebarsjs.com/guide/

我们向 Handlebars 评估器提供以下参数（将它们包裹在双大括号中）：

1. `description` - 角色的描述
2. `scenario` - 角色的场景
3. `personality` - 角色的个性
4. `system` - [指令模式] 系统提示或角色的主要提示覆盖（如果存在并且用户设置中启用了“优先角色提示”）
5. `persona` - 选定的角色描述
6. `char` - 角色的名字
7. `user` - 选定的角色名字
8. `wiBefore` 或 `loreBefore` - 位置设置为“角色定义之前”的组合激活的世界信息条目
9. `wiAfter` 或 `loreAfter` - 位置设置为“角色定义之后”的组合激活的世界信息条目
10. `mesExamples` - （可选）角色的示例对话，使用分隔符进行指令格式化。将“示例消息行为”设置为“从不包含示例”以避免重复。

支持一个特殊的 \{\{trim\}\} 宏，用于删除其周围的任何换行符。如果您希望某部分文本与前一行不分开，请使用它（_空格 **不会** 被修剪_）。

**警告**：如果故事字符串模板中缺少上述某些参数，它们将根本不会发送到提示中。

#### 示例分隔符

用作块头和示例对话块之间的分隔符。示例对话中的任何 `<START>` 标签实例将被此字段的内容替换。

#### 聊天开始

作为渲染的故事字符串和示例对话块之间的分隔符插入，但在上下文中的第一条消息之前。

### 用作停止字符串

将“示例分隔符”和“聊天开始”添加到停止字符串列表中。

如果模型倾向于幻觉或泄漏整个示例对话块，前面有分隔符，这将很有帮助。

### 允许后历史指令

在提示的末尾包含后历史指令，格式为最后的用户消息。

后历史指令提示应在角色卡中定义，并且应启用“偏好角色指令”设置。

应谨慎使用，因为将指令放在上下文的较低位置可能会导致较小模型的输出质量下降。

#### 始终将角色名称添加到提示中

将角色的名称附加到提示中，以强制模型作为该角色完成消息：

```
** 其他上下文在这里 **
角色：
```

### Tokenizer

Tokenizer 是一个将一段文本分解为称为 tokens 的更小单元的工具。这些 tokens 可以是单个单词，甚至是单词的部分，例如前缀、后缀或标点符号。一个经验法则是，一个 token 通常对应于 3~4 个字符的文本。

SillyTavern 提供了一个“最佳匹配”选项，该选项根据所使用的 API 提供者尝试匹配 tokenizer，遵循以下规则。

文本补全 API **(可覆盖)**：

1. NovelAI Clio：NerdStash tokenizer。
2. NovelAI Kayra：NerdStash v2 tokenizer。
3. 文本补全：API tokenizer（如果支持）或 LLaMA tokenizer。
4. KoboldAI Classic / AI Horde：LLaMA tokenizer。
5. Koboldcpp：模型 API tokenizer。

如果您获得不准确的结果或希望进行实验，您可以为 SillyTavern 设置一个 *覆盖 tokenizer*，以在向 AI 后端发出请求时使用：

1. 无。每个 token 的估计为 ~3.3 个字符，四舍五入到最接近的整数。**如果您的提示在高上下文长度下被截断，请尝试此方法。** 该方法由 KoboldAI Lite 使用。
2. LLaMA tokenizer。用于 LLaMA 1/2 模型系列：Vicuna、Hermes、Airoboros 等。**如果您使用 LLaMA 1/2 模型，请选择此项。**
3. NerdStash tokenizer。用于 NovelAI 的 Clio 模型。**如果您使用 Clio 模型，请选择此项。**
4. NerdStash v2 tokenizer。用于 NovelAI 的 Kayra 模型。**如果您使用 Kayra 模型，请选择此项。**
5. Mistral tokenizer。用于 Mistral 模型系列及其微调。**如果您使用 Mistral 模型，请选择此项。**
6. Yi tokenized。用于 Yi 模型。**如果您使用 Yi 模型，请选择此项。**
7. API tokenizer。查询生成 API 以直接从模型获取 token 计数。已知支持的后端：Text Generation WebUI (ooba)、koboldcpp、TabbyAPI、Aphrodite API。**如果您使用受支持的后端，请选择此项。**

聊天补全 API **(不可覆盖)**：

1. GPT 通过 OpenAI / OpenRouter / Window：依赖于模型的 tokenizer，通过 [tiktoken](https://github.com/openai/tiktoken)。
2. Claude：依赖于模型的 tokenizer，通过 [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp)。
3. OpenRouter：其各自模型的 Llama 和 Mistral tokenizer。
4. Scale API：GPT-4 tokenizer。
5. AI21 API：GPT-3.5 turbo（默认，快速）或 API tokenizer（如果切换打开，可能会导致速度变慢！）。
6. 回退 tokenizer（用于代理）：GPT-3.5 turbo tokenizer。

### Token Padding

**重要：本节不适用于 Chat Completions API。SillyTavern 将始终为这些模型使用匹配的分词器。**

SillyTavern 无法使用由在远程实例的 KoboldAI 或 Oobabooga 的 TextGen 上运行的模型提供的适当分词器，因此在提示生成过程中假定的所有令牌计数都是基于所选的 [tokenizer](#tokenizer) 类型进行估算的。

由于在接近模型定义的最大上下文大小时，分词的结果可能不准确，因此提示的某些部分可能会被修剪或删除，这可能会对角色定义的连贯性产生负面影响。

为了防止这种情况，SillyTavern 将上下文大小的一部分分配为填充，以避免添加超过模型可以容纳的聊天项。如果您发现即使选择了最匹配的分词器，提示的某些部分仍被修剪，请调整填充以确保描述不被截断。

您可以输入负值进行反向填充，这样可以分配超过设定的最大令牌数量。

## 自定义停止字符串

接受一个 JSON 序列化的停止字符串数组。示例：`["\n", "\nUser:", "\nChar:"]`。如果您不确定格式，请使用 [在线 JSON 验证器](https://jsonlint.com/)。

支持的 API：

1. KoboldAI（版本 1.2.2 及更高）或 KoboldCpp
2. AI Horde
3. 文本补全 API：文本生成 WebUI (ooba)、Tabby、Aphrodite、Mancer、TogetherAI、Ollama 等
4. NovelAI
5. OpenAI，包括通过 OpenRouter（最多 4 个字符串）
6. Claude
7. Google MakerSuite