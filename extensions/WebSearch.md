# 网络搜索

将网络搜索结果添加到 LLM 提示中。

## 可用资源

### Selenium 插件

需要安装并启用官方服务器插件。

有关更多详细信息，请参见 [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium)。

支持 Google 和 DuckDuckGo 引擎。

### Extras API

需要在主机上安装 `websearch` 模块和 Chrome/Firefox 网页浏览器。

支持 Google 和 DuckDuckGo 搜索引擎。

### SerpApi

需要 SerpApi 密钥，并提供对 Google 搜索的访问。

在此获取密钥： https://serpapi.com/dashboard

## 如何使用

1. 确保您使用的是最新版本的 SillyTavern。
2. 通过 SillyTavern 中的“下载扩展和资源”菜单安装扩展。
3. 打开“网络搜索”扩展设置，设置您的 API 密钥或连接到 Extras，并启用扩展。
4. 网络搜索结果将在您聊天时自然地添加到提示中。**仅用户消息会触发搜索。**
5. 要更自然地包含搜索结果，请用单个反引号包裹搜索查询：```Tell me about the `latest Ryan Gosling movie`.``` 将生成搜索查询 `latest Ryan Gosling movie`。
6. 可选地，根据您的喜好配置设置。

## 设置

### 一般

1. 启用 - 切换扩展的开启和关闭。
2. 来源 = 设置搜索结果的来源。
3. 缓存生命周期 - 搜索结果为您的提示缓存的时间（以秒为单位）。默认 = 一周。

### 提示设置

1. 提示预算 - 设置插入文本的最大容量（以字符为单位，而不是标记）。经验法则：1 个标记约为 3-4 个字符，根据模型的上下文限制进行调整。默认值 = 1500 个字符。
2. 插入模板 - 结果如何插入到提示中。支持常规宏 + 特殊宏：\{\{query\}\} 用于搜索查询，\{\{text\}\} 用于搜索结果。
3. 注入位置 - 结果在提示中的位置。与作者注释相同的选项：在聊天中注入或在系统提示之前/之后。

### 搜索激活

1. 使用反引号 - 通过用单个反引号括起来的词启用搜索激活。
2. 使用触发短语 - 通过触发短语启用搜索激活。
3. 触发短语 - 添加将触发搜索的短语，逐个添加。它可以出现在消息的任何位置，查询从触发词开始，延伸到“最大词数”的总数。要排除特定消息不被处理，必须以句点开头，例如 `.What do you think?`。触发器的优先级：首先按文本框中的顺序，其次是用户消息中的第一个。
4. 最大词数 - 包含在搜索查询中的单词数量（包括触发短语）。Google 每个提示的限制约为 32 个单词。默认 = 10 个单词。

### 页面抓取

1. 访问链接 - 将从访问的搜索结果页面提取文本并保存到文件附件中。
2. 访问计数 - 将访问和解析多少个链接以提取文本。
3. 访问域名黑名单 - 要排除访问的网站域名。每行一个。
4. 文件头 - 文件头模板，插入在文本文件的开头，包含额外的 \{\{query\}\} 宏。
5. 块头 - 链接块模板，插入每个链接解析的内容。使用 \{\{link\}\} 宏表示页面 URL，使用 \{\{text\}\} 表示页面内容。

## 更多信息

最新查询的搜索结果将保留在提示中，直到找到下一个有效查询。
如果您想提出额外的问题而不小心触发搜索，请以句点开头您的消息。

如果同时使用反引号和触发短语搜索激活，则反引号具有更高的优先级。

要丢弃所有先前的查询处理，请以感叹号开头用户消息，例如，用户消息 `!现在让我们谈谈...` 将丢弃此消息及其以上的所有消息。

此扩展还提供了一个 `/websearch` 斜杠命令可在 STscript 中使用。更多信息请查看：[STscript 语言参考](https://docs.sillytavern.app/usage/st-script/)

```
/websearch (links=on|off snippets=on|off [query]) – performs a web search query. Use named arguments to specify what to return - page snippets (default: on), full parsed pages (default: off) or both.

Example: /websearch links=off snippets=on how to make a sandwich
```

### 搜索结果中可以包含什么？

#### SerpApi

1. 答案框。对问题的直接回答。
2. 知识图谱。关于主题的百科知识。
3. 页面摘要（最多 10 个）。来自网页的相关摘录。
4. 相关问题（最多 10 个）。类似主题的问题和答案。

#### Selenium 插件和扩展 API

1. Google - 答案框、知识图谱、页面摘要。
2. DuckDuckGo - 页面摘要。