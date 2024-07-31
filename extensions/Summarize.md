# 总结

## 它是什么？

此扩展允许您根据聊天中发生的事件创建、存储和利用自动生成的摘要。摘要可以帮助概述故事中发生的总体细节，这*可能*被解读为长期记忆，但对此说法需谨慎。由于摘要是由语言模型生成的，输出可能会丢失一些重要细节或包含幻觉，因此建议您始终跟踪摘要状态，并在必要时手动进行更正。

## 常见配置

总结扩展默认安装在SillyTavern中，因此它会在ST的扩展面板（堆叠立方体图标）列表中显示如下：

![Summarize Config Panel](/static/extensions/summarize.png)

- **当前摘要** - 显示并提供修改当前摘要的功能。摘要会被更新并嵌入到聊天文件的元数据中，针对生成摘要时上下文中的最后一条消息。从聊天中删除或编辑有摘要附加的消息，将恢复到最后有效的摘要状态。
- **恢复之前的摘要** - 移除当前摘要，回滚到之前的状态。如果摘要生成器在某个时刻表现不佳，这个功能非常有用。
- **暂停** - 勾选此项可防止摘要被自动更新。如果您想提供自定义摘要或通过清除框并停止更新有效地禁用摘要，这个功能非常有用。
- **弹出窗口** - 允许将摘要分离到侧边栏的可移动UI面板中。对于桌面布局来说，这样可以轻松访问摘要设置，而无需通过扩展菜单进行导航。
- **注入模板** - 定义摘要在插入常规聊天提示时的包装方式。应使用特殊的 \{\{summary\}\} 宏来标记提示注入文本中当前摘要状态的确切位置。
- **注入位置** - 设置提示注入的位置。选项与作者笔记相同：在主提示之前或之后，或在指定深度的聊天中。

## 支持的摘要来源

### 主要 API

摘要将由您当前选择的 AI 后端、模型和设置提供支持。此方法无需额外设置，只需一个有效的 API 连接即可。

此选项具有以下子模式，具体取决于摘要提示的构建方式：

1. 原始，阻塞。摘要将仅使用摘要提示和聊天历史生成。后续提示还将包括生成摘要后发送的消息和之前的摘要（见示例）。这种模式可以（并且会）生成具有很大变异性的提示，因此不建议在处理提示速度较慢的后端中使用，例如 llama.cpp 及其衍生版本。
2. 原始，非阻塞。与上述相同，但在生成摘要时聊天生成不会被阻塞。并非所有后端都支持同时请求，因此如果摘要生成失败，请切换到阻塞模式。
3. 经典，阻塞。摘要提示将在您通常的生成提示结束时发送，作为中立的系统指令，不省略角色卡、主要提示、示例对话和聊天提示的其他部分。这通常会导致与重新使用已处理提示良好配合的提示，因此建议与 llama.cpp 及其兄弟版本一起使用。

#### 摘要设置说明

1. **摘要提示** - 定义用于创建摘要的提示。可以包含任何已知宏，以及一个特殊的 \{\{words\}\} 宏（见下文）。
2. **目标摘要长度（字数）** - 定义可以插入到摘要提示中的 \{\{words\}\} 宏的值。此设置完全是可选的，如果不使用该宏，则没有任何效果。
3. **API 响应长度（令牌）** - 允许设置用于生成与全局设置值不同的摘要的覆盖 API 响应长度。
4. **每个请求的最大消息数 _(仅原始模式)_** - 设置以限制将包含在一个摘要提示中的最大消息数。`0` 表示没有明确限制，但要摘要的消息数仍将取决于最大上下文大小，计算公式为：`max summary buffer = context size - summarization prompt - previous summary - response length`。当您希望在具有大上下文大小的模型上获得更集中的摘要时，请使用此项。
5. **无 WI/AN** - 从要摘要的文本中省略世界信息和作者说明。仅在使用经典提示构建器时有效。原始提示构建器始终省略 WI/AN。
6. **每 X 条消息更新** - 设置生成摘要的间隔。`0` 表示自动摘要已禁用，但您仍然可以通过单击“立即摘要”按钮手动触发它。根据提示缓冲区完全填满聊天消息的速度进行调整。理想情况下，当消息开始被丢弃时，您希望生成第一个摘要。
7. **每 X 个字更新** - 与上述相同，但使用字（而不是令牌！）而不是消息，理论上可以更准确地衡量，因为聊天消息的内容通常是不可预测的，但实际效果可能会有所不同。

如果两个“每个更新”滑块都设置为非零值，则两个滑块将根据先发生的情况在各自的间隔触发摘要更新。强烈建议在切换到具有不同上下文大小的其他模型时相应地更新这些值，否则摘要生成可能会触发得太频繁，或根本不触发。

如果您不确定间隔设置，可以单击“魔法棒”按钮，以尝试根据一些简单的启发式规则猜测最佳值。算法的简要描述如下：

1. 计算所有聊天消息的令牌和字数
2. 根据所需的提示字数确定目标摘要长度
3. 根据平均消息长度计算可以适应提示的最大消息数
4. 如果设置了“最大消息数”，则调整平均值以考虑不符合摘要限制的消息
5. 将调整后的每个提示的平均消息数向下舍入到 5 的倍数

#### 示例提示

**原始提示**
```
系统：
[摘要提示]

之前的摘要。

用户：
消息 foo。

角色：
消息 bar。
```

**经典提示**
```
[主要提示]

[角色卡]

[示例对话]

用户：
消息 foo。

角色：
消息 bar。

系统：
[摘要提示]
```

### Extras API

Extras 服务器与 `summarize` 模块可以运行辅助摘要模型 (BART)。

它的上下文大小非常小 (~1024 tokens)，因此处理大型摘要的能力相当有限。

要配置 Extras 摘要源，请执行以下操作：

1. 安装或更新 [Extras](https://github.com/SillyTavern/SillyTavern-extras) 到最新版本。
2. 运行启用 `summarize` 模块的 Extras： `python server.py --enable-modules=summarize`

#### 更改摘要模型

默认情况下，Summarize 使用 [Qiliang/bart-large-cnn-samsum-ChatGPT_v3](https://huggingface.co/Qiliang/bart-large-cnn-samsum-ChatGPT_v3) 模型进行摘要目的。

可以通过使用命令行参数 `--summarization-model=(###Hugging-Face-Model-URL-Here###)` 来更改此设置。

一个已知的替代 Summarize 模型是 `Qiliang/bart-large-cnn-samsum-ElectrifAi_v10`。