# 世界信息

**世界信息（也称为故事书或记忆书）增强了人工智能对您虚构世界细节的理解。**

它像一个动态字典，只在消息文本中出现与条目相关的关键词时插入相关的世界信息条目。

SillyTavern 引擎激活并无缝集成适当的背景知识到提示中，为人工智能提供背景信息。

*需要注意的是，虽然世界信息有助于引导人工智能朝您期望的背景知识方向发展，但并不能保证它会出现在生成的输出消息中。*

### 专业提示

* AI不会将关键词插入上下文，因此每个世界信息条目都应该是一个全面、独立的描述。
* 为了创建丰富而详细的世界背景，条目可以相互链接并相互引用。
* 为了节省令牌，建议保持条目内容简洁。
* 世界信息引擎是一个非常强大的提示管理工具。不要仅仅专注于添加角色背景，欢迎进行实验。

### 深入阅读

* [世界信息百科全书](https://rentry.co/world-info-encyclopedia): 关于世界信息和背景书的详尽深入指南。作者：kingbri, Alicat, Trappu。

## 角色背景

可选地，可以为一个角色分配一个世界信息文件，以作为该角色在所有聊天（包括群组）中的专用背景资料来源。

为此，导航到角色管理面板，点击地球按钮，然后从下拉列表中选择世界信息，最后点击“确定”。

### 角色背景插入策略

在生成AI回复时，角色世界信息的条目将与全球世界信息选择器的条目结合，使用以下策略之一：

#### 均匀排序（默认）

所有条目将根据它们的插入顺序进行排序，就好像它们是一个大文件的一部分，忽略来源。

#### 角色背景优先

角色世界信息的条目将首先按其插入顺序包含，然后是全球世界信息的条目。

#### 全球背景优先

全球世界信息的条目将首先按其插入顺序包含，然后是角色世界信息的条目。

### 世界信息条目

#### 关键字

触发世界信息条目激活的关键字列表。默认情况下，关键字不区分大小写（这是[可配置的](#case-sensitive-keys)）。

##### 正则表达式（Regex）作为关键字

关键字允许通过支持正则表达式来采用更灵活的匹配方式。这使得可以匹配更动态的内容，包括可选的单词或字符、空格以及正则表达式提供的所有其他工具。  
如果定义的关键字是有效的正则表达式（Javascript正则表达式风格，以`/`作为分隔符，所有标志均可使用），在检查条目是否应该被触发时将被视为正则表达式。可以将多个正则表达式作为单独的关键字输入，并且它们可以相互配合。在正则表达式内部，可以使用逗号。普通文本关键字不支持逗号，因为它们被视为关键字分隔符。  

高级正则匹配的一个使用案例示例：  
在字符进行天气相关操作时应插入的条目/指令
```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

有关正则表达式语法和可能性的更多信息：[正则表达式 - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

##### 关键字输入

有两种模式可以输入关键字，每种模式的用户界面略有不同。在*纯文本模式*（默认）中，关键字可以在单个文本框中作为用逗号分隔的列表输入。正则表达式也可以包含在内，但没有任何特殊的高亮显示。在*花哨模式*中，关键字作为单独的元素出现，正则表达式将被高亮显示。该控件支持编辑和删除关键字。可以通过输入控件内的内联按钮切换模式。

#### 可选过滤器

与主要关键字结合使用的补充关键字列表。请参见[可选过滤器](#optional-filter-1)。这些关键字也支持[正则表达式](#regular-expression-regex-as-keys)。

#### 条目内容

在条目激活时插入到提示中的文本。

#### 插入顺序

数值。如果多个条目同时被激活，定义条目的优先级。顺序号较高的条目将更靠近上下文的末尾插入，因为它们对输出的影响更大。

#### 插入位置

* **在角色定义之前：** 世界信息条目在角色的描述和场景之前插入。对对话有中等影响。
* **在角色定义之后：** 世界信息条目在角色的描述和场景之后插入。对对话有更大的影响。
* **在示例消息之前：** 世界信息条目作为示例对话块解析，并在角色卡提供的示例之前插入。
* **在示例消息之后：** 世界信息条目作为示例对话块解析，并在角色卡提供的示例之后插入。
* **在作者注释顶部：** 世界信息条目在作者注释内容的顶部插入。根据作者注释的位置，影响程度可变。
* **在作者注释底部：** 世界信息条目在作者注释内容的底部插入。根据作者注释的位置，影响程度可变。
* **@ D：** 世界信息条目在聊天中的特定深度插入（深度0为提示的底部）。
    - ⚙️ - 作为系统角色消息
    - 👤 - 作为用户角色消息
    - 🤖 - 作为助手角色消息

示例消息条目将根据提示构建设置进行格式化：指令模式或聊天完成提示管理器。它们还遵循示例消息行为规则：在完整上下文中逐渐被推送出去，始终保留或完全禁用。

如果您的作者注释被禁用（插入频率= 0），则作者注释位置的世界信息条目将被忽略！

#### 条目标题 / 备忘录

一个文本字段，方便您标记条目，AI或任何触发逻辑都不会使用该字段。

如果为空，可以通过单击“填充空备忘录”按钮使用条目的第一个关键字进行填充。

#### 状态

1. 🔵（蓝色圆圈）= 条目将始终出现在提示中。
2. 🟢（绿色圆圈）= 条目仅在存在关键字时触发。
3. 🔗（链条链接）= 条目允许通过嵌入相似性插入。
4. ❌（红色叉）= 条目已禁用。

#### 可选过滤器

与主要关键字结合使用的附加关键字的逗号分隔列表。  
如果未提供任何参数，则此标志将被忽略。  
支持AND ANY、NOT ANY或NOT ALL的逻辑。

1. AND ANY = 仅在主要关键字和任何一个可选过滤器关键字在扫描上下文中时激活条目。
2. AND ALL = 仅在主要关键字和所有可选过滤器关键字都存在时激活条目。
3. NOT ANY = 仅在主要关键字和没有任何可选过滤器关键字在扫描上下文中时激活条目。
4. NOT ALL = 如果所有可选过滤器在扫描上下文中，则阻止激活条目，尽管触发了主要关键字。

#### 概率

该值充当附加过滤器，在条目通过任何方式激活时（常量、主要关键字、递归），增加该条目不被插入的机会。

1. 概率= 100意味着条目将在每次激活时插入。
2. 概率= 50意味着条目将以1:1的机会插入。
3. 概率= 0意味着条目将不被插入（实际上禁用它）。

使用此功能在聊天中创建随机事件。例如，每条消息都有1%的机会唤醒一个古老的神，如果在消息中提到它的名字。

#### 包含组

包含组控制在多个具有相同组标签的条目同时被触发时如何选择条目。如果多个具有相同组标签的条目被激活，只有一个将插入到提示中。

默认情况下，选择的条目是根据其组权重随机选择的（默认是100分）——数字越高，选择的概率越高。这允许在触发的条目中进行随机选择，为交互增添了一定的惊喜和多样性。

单个条目可以属于多个包含组，如果它们被定义为逗号分隔的列表。上述逻辑同样适用。如果该条目被触发，它将*禁用*所有其他属于其任何组的条目。因此，如果激活了任何组，该条目将不会被激活。

#### 优先包含

为了提供更多控制，通过[包含组](https://docs.sillytavern.app/usage/core-concepts/worldinfo/#inclusion-group)激活哪些条目，您可以使用“优先包含”设置。此选项允许您确定性地选择哪个条目，而不是随机滚动组权重机会。

如果多个具有相同组标签且此设置开启的条目被激活，将选择“顺序”值最高的那个。这对于通过包含组创建回退序列非常有用。例如，优先选择低深度条目以获得更多强调，或者在两个都有效的情况下选择设置场景的特定指令。

#### 使用组评分

当此设置在全局或每个条目中启用时，激活的条目关键字的数量决定了组胜利者的选择。只有具有最高关键字匹配数量的组子集将被允许通过组权重或包含优先级激活——其余将被禁用并从组中移除。

使用此功能为大型组中的单个条目提供更多特异性。例如，它们可以具有一个共同的关键字和一个特定的关键字。当未提供特定关键字时，将插入随机条目，反之亦然。

主要关键字的评分计算逻辑为1个匹配= 1分。

对于次要关键字，交互取决于所选的选择逻辑：

1. AND ANY：1个次要匹配= 1分。
2. AND ALL：如果所有次要关键字都匹配，则每个次要关键字得1分。
3. NOT ANY和NOT ALL：没有变化。

示例：

- 条目 1. 关键字：song, sing, Black Cat。组：songs
- 条目 2. 关键字：song, sing, Ghosts。组：songs

输入`sing me a song`可以激活任意条目（两个都激活了2个关键字），但`sing me a song about Ghosts`将仅激活条目2（激活了3个关键字）。

#### 自动化ID

允许将世界信息条目与[ST脚本](https://docs.sillytavern.app/usage/st-script/)从快速回复扩展集成。如果快速回复命令和世界信息条目具有相同的自动化ID，则在激活具有匹配ID的条目时，该命令将自动执行。

自动化将按照触发的顺序执行，遵循您指定的排序策略，将[角色背景插入策略](#character-lore-insertion-strategy)与“优先级”排序结合在一起。这导致[蓝色圆圈](#status)条目首先被处理，随后是其他条目，按其指定的“顺序”处理。递归触发的条目将在相同顺序后处理。

如果多个具有相同自动化ID的条目被激活，脚本命令将仅运行一次。

## 向量存储匹配

向量存储扩展通过使用最近聊天消息与世界信息条目内容之间的相似性，提供了一种替代关键字匹配的方法。

要启用并使用此功能，需要满足以下先决条件：

1. 向量存储扩展已启用，并配置为使用可用的嵌入源之一。
2. 在向量存储扩展设置中勾选“为世界信息启用”复选框。
3. 允许无关键字匹配的世界信息条目具有“向量化”（🔗）状态，或者在向量存储设置中勾选“为所有条目启用”选项。

扩展中的向量化模型选择和“嵌入”一词背后的理论含义在此不作讨论。如果您需要更多关于此主题的信息，请查看[数据银行](https://docs.sillytavern.app/usage/core-concepts/data-bank/#vector-storage)指南。

向量存储匹配遵循以下规则：

- 允许与向量存储匹配的最大条目数可以通过“最大条目”设置进行调整。此数字仅设置限制，不影响在世界信息的激活设置中设定的令牌预算。所有预算规则仍然适用。
- 此功能仅替代对关键字的检查。条目插入仍需满足所有附加检查：触发器%、角色过滤器、包含组等。
- 不使用激活设置或条目覆盖中的“扫描深度”设置。改为使用向量存储的“查询消息”值来获取匹配文本。这允许将“扫描深度”配置为0，因此不会进行常规关键字匹配，但条目仍然可以通过向量激活。
- “向量化”状态仅是一个附加标记。该条目仍会像正常的、启用的、非恒定记录一样，如果设置了关键字，则将通过关键字激活。如果希望条目仅通过向量激活，请移除关键字。

!!! info 注意
由于检索质量完全依赖于嵌入模型的输出，因此无法准确预测将插入哪些条目。如果您希望获得确定性和可预测的结果，请坚持使用关键字匹配。
!!!

## 定时效果

通常，世界信息评估是无状态的，这意味着评估的结果是相同的，仅依赖于当前的聊天上下文。然而，随着定时效果的引入，您可以创建具有激活延迟的条目、在触发后保持激活的条目，或在激活后无法触发的条目。

### 定时效果规则

1. 效果的时间框架以消息为单位进行测量（而不是消息对/交换），0表示没有效果。
2. 效果仅适用于激活条目的聊天。分支继承父聊天的状态。
3. 如果聊天没有进展，主动定时效果将被移除，例如，如果最后一条消息被滑动或删除。
4. 对当前处于定时效果中的条目进行任何更改将导致效果被强制移除。
5. 连续触发关键词不会刷新效果持续时间，如果它已经处于激活状态。

### 定时效果的类型

1. Sticky - 条目在激活后会在 N 条消息中保持有效。粘性条目在过期之前会忽略后续扫描中的概率检查。
2. Cooldown - 条目在激活后 N 条消息内无法被激活。可以与粘性一起使用：当粘性持续时间结束时，条目进入冷却状态。
3. Delay - 条目无法被激活，除非在评估时聊天中至少有 N 条消息。
    * Delay = 0 -> 条目可以在任何时候被激活。
    * Delay = 1 -> 如果聊天为空（没有问候），则条目无法被激活。
    * Delay = 2 -> 如果聊天中没有消息或只有一条消息，则条目无法被激活，等等。

### 定时效果示例

条目配置：sticky = 3，cooldown = 2，delay = 2。

```txt
Message 0: delay
Message 1: entry activated
Message 2: sticky
Message 3: sticky
Message 4: sticky
Message 5: cooldown
Message 6: cooldown
Message 7: entry can be activated again
```

## 激活设置

在世界信息屏幕顶部的可折叠菜单。

### 扫描深度

> 可以在条目级别被覆盖。

定义在聊天历史中应扫描多少条消息以获取世界信息键。

* 如果设置为 0，则仅评估递归条目和作者注释。
* 如果设置为 1，则 SillyTavern 仅扫描最后一条消息。
* 2 = 最近两条消息，依此类推。

### 上下文 % / 预算

**定义了 World Info 条目一次可以使用多少个令牌。**
您可以定义相对于 API 的最大上下文设置的阈值（上下文 %）或一个目标令牌阈值（预算）

如果预算耗尽，则即使提示中存在键，也不会激活更多条目。

常量条目将首先插入。然后是具有更高顺序号的条目。

通过直接提及其键插入的条目优先级高于在其他条目内容中提及的条目。

### 最小激活

最小激活：如果设置为非零值，这将忽略“扫描深度”的限制，从最新消息开始向后搜索聊天记录中的关键字，直到触发的条目数量达到最小激活中指定的数量。这仍然会受到最大深度设置或您整体预算上限的限制。

*由最小激活触发的额外扫描将不会检查之前步骤中递归添加的条目。只有聊天消息和扩展提示可以触发这些额外的激活。然而，最小激活触发的条目可以像往常一样触发其他条目。*

### 最大深度

使用最小激活设置时扫描的最大深度。

### 递归扫描

递归扫描允许条目激活其他条目或被其他条目激活，从而实现不同世界信息条目之间的复杂交互和依赖关系。此功能可以显著增强您的叙事或角色扮演场景的动态特性。  
是否启用递归扫描可以通过全局设置**递归扫描**进行控制。  
每个条目的递归控制有三个选项：

- **不可递归**：当选中此复选框时，该条目将不会被其他条目激活。这对于不应该改变或受其他世界信息条目影响的静态信息非常有用。
  
- **防止进一步递归**：选择此选项可确保一旦激活该条目，就不会触发其他条目。这有助于避免意外的激活链。

- **延迟直到递归**：该条目仅在递归检查期间激活，这意味着它不会在首次扫描中被触发，但可以被启用递归的其他条目激活。这对于只有在特定引用其他条目时才应发挥作用的更深层次信息，或在激活其他内容时故意保留的信息非常有用。

**条目可以通过在内容文本中提及其关键字来激活其他条目。**

例如，如果您的世界信息包含两个条目：

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friends with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

**如果消息文本仅提到** Bessie **，那么这两个条目都将被拉入上下文。**

### 区分大小写的键

> 可以在条目级别被覆盖。

**要被拉入上下文，条目键需要与在 World Info 条目中定义的大小写匹配。**

当你的键是常见词或常见词的一部分时，这一点非常有用。

例如，当此设置处于活动状态时，键 'rose' 和 'Rose' 将根据输入被视为不同。

### 匹配整个单词

> 可以在条目级别覆盖。

仅当搜索文本中存在整个单词时，才会匹配仅包含一个单词的键的条目。默认情况下启用。

例如，如果该设置启用且条目键为 "king"，则文本 "long live the king" 将被匹配，但 "it's not to my liking" 则不会。

### 溢出警报

如果激活的世界信息超过分配的令牌预算，则显示警报。