---
tags:
    [
        向量存储,
        RAG,
        检索增强生成,
        向量,
        文档,
        文件,
        附件,
    ]
---

# 数据库 (RAG)

检索增强生成 (RAG) 是一种为 LLM 提供外部知识来源的技术。它通过访问模型训练数据之外的信息，帮助提高 AI 答案的准确性。

SillyTavern 提供了一套工具，用于从多种来源构建多用途知识库，以及在 LLM 提示中使用收集的数据。

## 访问数据银行

内置的聊天附件扩展（在版本 >= 1.12.0 的发布版本中默认包含）在“魔法棒”菜单中添加了一个新选项 - 数据银行。这是您在 SillyTavern 中管理可用于 RAG 的文档的中心。

## 关于文档

数据银行存储文件附件，也称为文档。文档分为三种可用范围。

1. 全球附件 - 在每个聊天中可用，无论是单人还是群组。
2. 角色附件 - 仅在当前选择的角色中可用，包括在他们回复群组时。_附件是本地保存的，不会与角色卡一起导出！_
3. 聊天附件 - 仅在当前打开的聊天中可用。聊天中的每个角色都可以从中提取。

!!! info 注意
虽然不正式属于数据银行，但您可以将文件附加到单独的消息中。使用“魔杖”菜单中的附加文件选项，或在消息操作行中的回形针图标。
!!!

什么可以成为文档？实际上任何可以用纯文本形式表示的东西！

示例包括但不限于：

- 本地文件（书籍、科学论文等）
- 网页（维基百科、文章、新闻）
- 视频转录本

各种扩展和插件也可以提供新的数据收集和处理方式，更多内容见下文。

## 数据源

要将文档添加到任何范围，请单击“添加”并选择可用的源之一。

### 记事本

从头创建一个文本文件，或编辑现有的附件。

### 文件

从计算机的硬盘上传文件。SillyTavern 提供了流行文件格式的内置转换器：

-   PDF（仅文本）
-   HTML
-   Markdown
-   ePUB
-   TXT

您还可以附加任何具有非标准扩展名的文本文件，例如 JSON、YAML、源代码等。如果所选文件类型没有已知的转换，并且该文件无法被解析为纯文本文档，则文件上传将被拒绝，这意味着不允许上传原始二进制文件。

!!! 信息 注意
导入 Microsoft Office（DOCX、PPTX、XLSX）和 LibreOffice 文档（ODT、ODP、ODS）需要安装并加载 [服务器插件](https://github.com/SillyTavern/SillyTavern-Office-Parser)。有关安装说明，请参见插件的 README 页面。
!!!

### 网络

通过其 URL 从网页中抓取文本。然后通过 [Readability](https://github.com/mozilla/readability) 库处理 HTML 文档，以提取仅可用的文本。

某些网络服务器可能会拒绝抓取请求，受到 Cloudflare 的保护，或严重依赖 JavaScript 才能正常工作。如果您在特定网站上遇到问题，请通过网页浏览器手动下载该页面，并使用文件上传器附加它。

### YouTube

通过视频的ID或URL下载YouTube视频的文字稿，文字稿可以是创作者上传的或由Google自动生成的。某些视频可能禁用了文字稿，此外，因需要登录，无法解析年龄限制的视频。

脚本以视频的默认语言加载。可选地，您可以指定两个字母的语言代码，尝试以特定语言获取文字稿。此功能并非总是可用，可能会失败，因此请谨慎使用。

### 网络搜索

!!! info 注意
此资源需要安装并正确配置 [Web Search](/Extras/extensions/WebSearch.md) 扩展。有关更多详细信息，请参见链接页面。
!!!

执行网络搜索并下载搜索结果页面中的文本。这类似于网络源，但完全自动化。所选择的搜索引擎将从扩展设置中继承，因此请提前进行设置。

要开始，请指定搜索查询、要访问的最大链接数量和输出类型：一个合并的文件（根据扩展规则格式化）或每个页面的单独文件。您还可以选择保存页面摘要。

### Fandom

!!! info 注意
此来源需要安装并加载 [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)。请参阅插件的README页面以获取安装说明。
!!!

通过其ID或URL从 [Fandom](https://www.fandom.com/) 维基抓取文章。由于某些维基非常庞大，限制使用过滤正则表达式的范围可能会很有益，它将针对文章的标题进行测试。如果未提供过滤器，则所有页面都将被导出。您可以将它们保存为每个页面的单独文件，或合并为一个文档。

### Bronie Parser Extension (第三方)

!!! warning 注意
此来源来自第三方，与 SillyTavern 团队 **无关**。此来源要求您安装 Bronya Rand 的 [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension)，以及需要解析器才能工作的服务器插件。
!!!

Bronya Rand 的 Bronie Parser Extension 允许将第三方抓取工具（例如 miHoYo/HoYoverse 的 [HoYoLab](https://wiki.hoyolab.com)）引入 SillyTavern，类似于其他数据来源。

目前，Bronya Rand 的 Bronie Parser Extension 支持以下内容：

-   miHoYo/HoYoverse 的 HoYoLab（用于原神/崩坏：星穹铁道）通过 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

要开始，请按照其 [安装指南](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation) 安装 Bronya Rand 的 Bronie Parser Extension，并在 SillyTavern 中安装一个受支持的服务器插件。重启 SillyTavern，进入 _数据银行_ 菜单。点击 `+ 添加`，您应该会看到最近安装的抓取工具已被添加到可获取信息的来源列表中。

## 向量存储

所以，您已经为您的特定主题构建了一个完善的信息库。接下来该怎么办？

要将文档用于 RAG，您需要使用一个兼容的扩展，它将相关数据插入到 LLM 提示中。

向量存储，作为 SillyTavern 的一部分，是此类扩展的参考实现。它使用嵌入（也称为向量）来搜索与您正在进行的聊天相关的文档。

!!! info 趣事

1. 嵌入是数字数组，抽象地表示一段文本，由专业语言模型生成。相似的文本之间的向量距离较短。
2. 向量存储扩展使用 [Vectra](https://github.com/Stevenic/vectra) 库来跟踪文件嵌入。它们存储在您用户数据目录的 `/vectors` 文件夹中的 JSON 文件中。每个文档在内部由其自己的索引/集合文件表示。
   !!!

由于向量功能默认是禁用的，您需要打开扩展面板（顶部栏上的“堆叠立方体”图标），然后导航到“向量存储”部分，并在“文件向量化设置”下勾选“启用文件”复选框。

向量存储本身不会生成任何向量，您需要使用一个兼容的嵌入提供者。

## 向量提供者

### 本地

这些来源是免费的，且没有限制，使用您的 CPU/GPU 来计算嵌入。

1. 本地 (Transformers) - 在 Node 服务器上运行。SillyTavern 会自动从 HuggingFace 下载兼容的 ONNX 格式模型。默认模型：[jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en)。
2. 附加功能 - 在 [Extras API](https://github.com/SillyTavern/SillyTavern-extras) 下运行，使用 SentenceTransformers 加载器。默认模型：[all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)。
3. Ollama - 从 <https://ollama.com/> 获取。在 API 连接菜单中设置 API URL（在文本补全下，默认：`http://localhost:11434`）。必须先下载兼容的模型，然后在扩展设置中设置其名称。示例模型：[mxbai-embed-large](https://ollama.com/library/mxbai-embed-large)。可选，勾选一个选项以保持模型加载在内存中。
4. llama.cpp 服务器 - 从 [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) 获取，并使用 `--embedding` 标志运行服务器可执行文件。从 HuggingFace 加载兼容的 GGUF 嵌入模型，例如 [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF)。
5. vLLM - 从 [vllm-project/vllm](https://github.com/vllm-project/vllm) 获取。首先在 API 连接菜单中设置 API URL 和 API 密钥。

### API 源

所有这些源都需要相应服务的 API 密钥，并且通常会有使用费用，但一般来说，计算嵌入的成本相对较低。

1. OpenAI
2. Cohere
3. Google MakerSuite
4. TogetherAI
5. MistralAI
6. NomicAI

!!! warning 警告
嵌入仅在使用生成它们的相同模型检索时可用。更改嵌入模型或源时，建议清除并重新计算文件向量。
!!!

## 向量化设置

在选择了嵌入提供者之后，请不要忘记配置其他设置，这些设置将定义处理和检索文档的规则。

!!! info 注意
从附件中拆分、向量化和检索信息需要一些时间。虽然文件的初始摄取可能需要一段时间，但RAG搜索查询通常足够快速，不会造成显著的延迟。
!!!

### 消息附件

这些设置控制直接附加到消息中的文件。

以下规则适用：

1. 只有适合 LLM 上下文窗口的消息才能检索其附件。
2. 当向量存储扩展禁用时，文件附件及其附带的消息将完全插入到提示中。
3. 当文件向量化启用时，文件将被拆分成多个块，仅插入最相关的部分，从而节省上下文空间并使模型保持专注。

-   大小阈值 (KB) - 设置块拆分的阈值。只有大于指定大小的文件才会被拆分。
-   块大小 (chars) - 设置单个块的目标大小（以文本字符为单位，而不是模型令牌！）。
-   块重叠 (%) - 设置相邻块之间共享的块大小的百分比。这允许块之间更平滑的过渡，但也可能引入一些冗余。
-   检索块 - 设置要检索的最相关文件块的最大数量。它们将按原始顺序插入。

### 数据库文件

这些设置控制如何处理数据库文档。

适用以下规则：

1. 当文件向量化被禁用时，数据库将不被使用。
2. 否则，当前范围内的所有可用文档（见上文）将被考虑用于查询。仅检索所有文件中最相关的片段。来自同一文件的多个片段将按其原始顺序插入。
3. 插入的片段将在适应聊天消息之前保留一部分上下文。

-   大小阈值 (KB) - 设置片段分割的阈值。只有大于指定大小的文件才会被分割。
-   片段大小 (chars) - 设置单个片段的目标大小（以文本字符计算，而不是模型标记！）。
-   片段重叠 (%) - 设置相邻片段之间共享的片段大小的百分比。这允许片段之间更平滑的过渡，但也可能引入一些冗余。
-   检索片段 - 设置要检索的文件片段的最大数量。此配额在所有文件之间共享。
-   注入模板 - 定义检索到的信息如何插入到提示中。您可以使用特殊的 \{\{text\}\} 宏来指定检索文本的位置，以及任何其他宏。
-   注入位置 - 设置插入提示注入的位置。适用与作者注释和世界信息相同的规则。

### 共享设置

-   查询消息 - 将使用多少最新的聊天消息来查询文档片段。
-   分数阈值 - 调整以允许根据相关性分数（0 - 完全不匹配，1 - 完美匹配）筛选片段的检索。较高的值允许更准确的检索，并防止完全随机的信息进入上下文。合理的值范围在 0.2（更宽松）和 0.5（更集中）之间。
-   包含在世界信息扫描中 - 检查是否希望注入的内容激活背景书条目。
-   全部向量化 - 强制摄取所有未处理文件的嵌入。
-   清除向量 - 清除文件嵌入，允许重新计算它们的向量。

!!! warning 警告
"聊天向量化"设置超出了本指南的范围，将不在此处涵盖。
!!!

## 结论

恭喜你！你的聊天体验现在得益于 RAG 的强大功能。它的能力仅受限于你的想象力。和往常一样，不要害怕尝试！