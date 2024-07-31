---
图标: 服务器
---

# 服务器插件

这些插件允许添加仅使用 UI 扩展无法实现的功能，例如创建新的 API 端点或使用在浏览器环境中不可用的 Node.JS 包。

插件位于 SillyTavern 的 `plugins` 目录中，并在服务器启动时加载，但*仅当* `config.yaml` 文件中的 `enableServerPlugins` 设置为 `true` 时。

> **插件没有沙箱，安装和运行时请小心！**

## 插件类型

### 文件

一个可执行的 JavaScript 文件，扩展名为 ".js"（用于 CommonJS 模块）或 ".mjs"（用于 ES 模块），包含一个导出 `init` 函数的模块，该函数接受一个为您的插件专门创建的 Express 路由器作为参数，并返回一个 Promise。

该模块还应导出一个 `info` 对象，包含有关插件的信息（`id`、`name` 和 `description` 字符串）。这将为加载器提供有关插件的信息。

您可以通过路由器注册路由，这些路由将注册在 `/api/plugins/{id}/{route}` 路径下。例如，插件 `example` 的 `router.get('/foo')` 将生成如下路由：`/api/plugins/example/foo`。

插件还可以 *可选地* 导出一个 `exit` 函数，在关闭服务器时执行清理。它不应有参数，并且必须返回一个 Promise。

插件导出的 TypeScript 合同：

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

以下是一个“Hello world！”插件示例：

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### 目录

您可以通过以下方式之一从 `plugins` 的子目录加载插件（按检查顺序）：

1. `package.json` 文件，其中包含 "main" 字段指向可执行文件的路径。
2. CommonJS 模块的 `index.js` 文件。
3. ES 模块的 `index.mjs` 文件。

结果文件必须导出一个 `init` 函数和一个 `info` 对象，其要求与单个文件相同。

目录插件示例（带有 `index.js` 文件）： <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### 打包

建议使用打包工具（如 Webpack 或 Browserify）将所有依赖项打包成一个文件。确保将“Node”设置为构建目标。

使用 Webpack 和 TypeScript 的插件模板库： <https://github.com/SillyTavern/Plugin-WebpackTemplate>