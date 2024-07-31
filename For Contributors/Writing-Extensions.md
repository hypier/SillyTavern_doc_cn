---
icon: file-added
---

# UI 扩展

UI 扩展通过连接 SillyTavern 的事件和 API 来扩展其功能。您可以轻松创建自己的扩展。

想将您的扩展贡献给 [官方仓库](https://github.com/SillyTavern/SillyTavern-Content) 吗？联系我们！

## 示例

查看简单的 SillyTavern 扩展的实时示例：

* https://github.com/city-unit/st-extension-example - 基本扩展模板。展示了清单创建、本地脚本导入、添加设置 UI 面板和持久扩展设置的使用。

## 打包

扩展还可以利用打包将自己与其他模块隔离，并使用来自 NPM 的任何依赖，包括像 Vue、React 等 UI 框架。

* https://github.com/SillyTavern/Extension-ReactTemplate - 使用 React 和 Webpack 的基础扩展模板库。

要从打包中使用相对导入，您可能需要创建一个导入包装器。以下是 Webpack 的一个示例：

```js
// define
async function importFromScript(what) {
    const module = await import(/* webpackIgnore: true */'../../../../../script.js');
    return module[what];
}

// use
const generateRaw = await importFromScript('generateRaw');
```

## manifest.json

每个扩展必须在 `data/<user-handle>/extensions` 中有一个文件夹，并且必须有一个包含插件元数据的 manifest.json 文件和一个 JS 脚本文件。

```js
{
    "display_name": "The name of the plugin",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/plugin",
    "auto_update": true
}
```

* `display_name` 是必需的。显示在“管理扩展”菜单中。
* `loading_order` 是可选的。数字越高，加载越晚。
* `requires` 指定所需的 Extras 模块依赖项。除非连接的 Extras API 提供所有这些模块，否则扩展将不会加载。
* `optional` 指定可选的 Extras 依赖项。
* `js` 是主要的 JS 文件引用，是必需的。
* `css` 是可选的样式文件引用。
* `author` 是必需的。应包含作者的姓名或联系信息。
* `auto_update` 如果扩展在 ST 包版本更改时应自动更新，则设置为 true。

可下载的扩展被挂载到 `/scripts/extensions/third-party` 文件夹中，因此应根据此使用相对导入。如果您计划从 GitHub 安装扩展，覆盖 `third-party` 文件夹中的内容，请注意在开发期间创建扩展的位置。

#### `requires` 与 `optional`

* `requires` - 扩展可以安装，但在用户连接到提供所有指定模块的 Extras API 之前不会加载。
* `optional` - 扩展可以安装并且将始终加载，但缺少的任何 Extras API 模块将在“管理扩展”菜单中突出显示。

要检查当前由连接的 Extras API 提供哪些模块，请从 `scripts/extensions.js` 导入 `modules` 数组。

### 使用 getContext

getContext() 函数让您可以访问 SillyTavern 上下文：

```js
import { getContext } from "../../extensions.js";

const context = getContext();
context.chat; // 聊天记录
context.characters; // 角色列表
context.groups; // 群组列表
// 还有更多...
```

使用这个与主应用状态进行交互。

您还可以使用全局定义的窗口对象：`SillyTavern.getContext()`。

### 从其他文件导入

除非您正在构建一个打包的扩展，否则可以从其他 JS 文件中导入变量和函数。

例如，这段代码片段将从当前选定的 API 生成回复：

```js
import { generateQuietPrompt } from "../../../script.js";

function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt(text);
    // ...
}
```

### 重要提示

从 SillyTavern 代码中使用导入是不可靠的，如果 ST 模块的内部结构发生变化，可能会随时中断。`getContext` 提供了一个更稳定的 API。

如果您在 `getContext` 中缺少任何函数/属性，请与开发者联系或向我们发送 Pull Request！

## 注册斜杠命令（新方式）

虽然 `registerSlashCommand` 仍然存在以兼容旧版本，但新的斜杠命令现在应通过 `SlashCommandParser.addCommandObject()` 注册，以便为解析器提供有关命令及其参数的扩展详细信息（进而用于自动完成和命令帮助）。

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

## (已弃用) 注册斜杠命令（旧方法）

使用 `registerSlashCommand()` 注册一个新的斜杠命令：

```js
import { registerSlashCommand } from "../../slash-commands.js";

registerSlashCommand("commandname", commandFunction, ["alias"], "描述在 /help 中显示");

function commandFunction(namedArgs, unnamedArgs) {
    // 命令逻辑
}
```

命令执行示例：
```
/commandname namedArgument=value 未命名参数
```

参数说明：

1. `command` - 主要命令名称。它将在自动补全和帮助命令中使用。
2. `callback` - 当命令被触发时将执行的函数。回调函数可以接受两个参数：
  * `namedArgs` - 一个包含命名参数的对象
  * `unnamedArgs` - 一个包含未命名参数的字符串
3. `aliases` - 一个包含别名字符串的数组。可以使用任何别名调用命令，但它们不会显示在自动补全中，而会在帮助命令中列出。
4. `helpString` - 当调用 `/help slash` 命令时将显示的字符串。必须描述您的命令的功能以及接受哪些参数。可以包含 HTML 标记。

所有注册的命令都可以在 [STscript](https://docs.sillytavern.app/usage/st-script/) 中以任何可能的方式使用。

> 在极少数情况下，未命名命令参数也可以接收一个数字，因此如果您期望具体类型，请务必进行类型检查或转换！

### 监听事件类型

使用 eventSource.on() 来监听事件：

```js
import { eventSource, event_types } from "../../../script.js";

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

主要的事件类型有：

* `MESSAGE_RECEIVED`
* `MESSAGE_SENT`
* `CHAT_CHANGED`

其余的可以在 [这里](https://github.com/SillyTavern/SillyTavern/blob/a410c63333069100aee6d4e4b54196cd6accfa24/public/script.js#L399) 找到。

### Do Extras 请求

`doExtrasFetch()` 函数允许您向 SillyTavern Extra 服务器发起请求。

例如，要调用 `/api/summarize` 端点：

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // Request body
    })
});
```

`getApiUrl()` 返回 Extras 服务器的基本 URL。

doExtrasFetch() 函数：

* 添加 Authorization 和 Bypass-Tunnel-Reminder 头部
* 处理结果的获取
* 返回结果（响应对象）

这使得从您的插件调用 Extra 的 API 变得简单。

您可以指定：

* 请求方法：GET、POST 等
* 额外的头部
* POST 请求的主体
* 以及其他任何获取选项