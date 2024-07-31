# CFG

页面作者：kingbri

贡献者：kingbri, Guillaume "Vermeille" Sanchez, AliCat

## 它是什么？

CFG，即无分类器引导，是一种用于帮助使提示的某些部分更突出或不那么突出的技术。

### 支持的后端 API

目前，支持的后端有 oobabooga 的 textgen WebUI、NovelAI 和 TabbyAPI。NovelAI 有自己的 CFG 文档，您可以在 [这里](https://docs.novelai.net/text/cfg.html) 阅读。

警告：由于处理超过 1 个提示，CFG 会增加显存使用量！如果您的 GPU 内存在生成启用 CFG 的提示时耗尽，请考虑减小上下文大小、使用参数较小的模型或完全关闭 CFG。

---

## 配置

访问 CFG 设置与访问作者注释相同：

![CFGhamburgermenupng](/static/cfg-hamburger.png)

以下是 CFG 面板的外观：

![CFGchatpanelpng](/static/cfg-panel.png)

CFG 面板中有四个下拉菜单：

- 聊天 CFG
  
  - 将 CFG 比例和提示限制在此聊天中
- 角色 CFG
  
  - 将 CFG 比例和提示限制在指定角色上
- 全局 CFG
  
  - 全局覆盖 CFG 比例和提示（也覆盖模型预设！）
- CFG 高级设置（以前称为 CFG 提示级联）
  
  - 用于组合前面 3 个下拉菜单中的提示并设置插入深度。

注意：如果指导比例设置为 1，则不会发送任何内容，因为这时 CFG 处于“关闭”状态。

#### 群组聊天

在群组聊天中，CFG 比例面板如下所示：

![CFGpanelgcpng](/static/cfg-groups.png)

主要变化是角色 CFG 被移除，并且在聊天 CFG 下拉菜单中出现了一个名为 `使用角色 CFG 比例` 的复选框。这允许使用当前角色的指导比例，而不是聊天 CFG 比例的设置。

此功能的主要用途是根据每个角色的个体需求调整比例。

此外，在提示级联中勾选 `角色负面` 复选框将附加独立的角色负面提示以及聊天提示（如果启用）。

## 概念

### 这不是在 Stable Diffusion 中吗？

是的，也不是。与在 Stable Diffusion 中可能习惯的方式不同，LLM 中的 CFG 工作原理是基于“提示混合”。CFG 公式采用正向和负向提示，然后混合它们之间的 *差异*。从那里，发送一个组合提示并生成响应！

这里有一个插图来帮助可视化这个概念。红色代表负向提示，蓝色代表中性提示，紫色代表被解释的混合结果。所有的空白区域在三个提示中都是相同的，因此这些区域不用于 CFG 混合。

![stcfgdiagrampng](/static/cfg-diagram.png)

如果您想了解更多关于 CFG 和 LLM 的信息，Vermifuge 的原始论文在这里。我建议您阅读/收听一下：

- 论文 - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- 音频版本 - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)

### 我需要 CFG 提示吗？

不需要！CFG 提示是完全可选的。只需将指导比例调整到 `1` 以上，也可以对响应产生影响，从而增强聊天和角色互动。

### 什么是好的 CFG 提示？

所以，我们已经确定 CFG 提示与 Stable Diffusion 的负标签和嵌入不同。我们该如何创建提示？

警告：这假设您已经使用 PLists 和 Ali:Chat 创建了一个角色。如果没有，请随意尝试各种提示技巧。

假设我有一个名叫“John”的角色。根据他的示例对话，John 应该总是感到快乐和兴奋。然而，在与 John 聊天时，他有时会感到悲伤和沮丧。

为了解决这个问题，CFG 来救援了！只需使用负提示 `[John's feelings: sad, depressed]` 来帮助去除悲伤的部分。您还可以选择使用正提示 `[John's feelings: happy, joyful]` 进一步突出 John 的快乐部分。

### 积极提示

我在上一节中提到过这一点，但我想再多谈谈。积极提示用于进一步强调角色的某些部分。我们再用约翰作为例子。通过使用积极提示 `[John's feelings: happy, joyful]` 使他更快乐，约翰应该开始输出带有更快乐感觉的对话，而不是不包含积极提示的情况。

### 但是...

这些只是基于某种特定字符格式的**宽松指南**。还有许多其他创建提示的方法，您应该进行尝试。欢迎与其他用户分享您的想法！

### 指导比例

这是一个经验法则。指导比例为 `1` 意味着 CFG 被禁用。实际上，如果指导比例为 1，SillyTavern 将不会向您的后端发送任何内容。指导比例 `>1` 将在不同程度上给出其他部分所示的结果。

然而，指导比例 `<1` 将产生相反的效果，因为负提示在这里被用作主要提示。

我们再用约翰的例子。负提示是 `[John's feelings: sad, depressed]`，正提示是 `[John's feelings: happy, joyful]`，指导比例为 `0.8`。

这将反过来更加突出负提示，您会看到约翰开始表现得比平常更悲伤，而不是更快乐。

tldr; 使用 `1.5` 的指导比例，并根据您的输出在此基础上上下调整。

### 提示级联

负面和正面可以在CFG类型之间级联（类型包括每个聊天、每个角色和全局覆盖）。有关更多信息，请参见配置标题。

### 插入深度

遵循基本规则：在提示中位置越低的内容，对响应的影响越大。对于聊天，我建议使用默认深度 `1`，因为它与SillyTavern的其他组件非常灵活。

但是，如果你想进行实验，可以使用插入深度 `0`。不过，这可能会显著改变你的响应外观，不建议在这里使用提示级联！