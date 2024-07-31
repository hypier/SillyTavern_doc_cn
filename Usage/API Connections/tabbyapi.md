# TabbyAPI
一个基于 FastAPI 的应用程序，允许使用 Exllamav2 后端生成文本，支持 Exl2、GPTQ 和 FP16 模型。

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### 快速开始
1. 按照官方 TabbyAPI GitHub 上的 [安装说明](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started)。
2. [创建你的 config.yml](https://github.com/theroyallab/tabbyAPI/wiki/2.-Configuration) 来设置你的模型路径、默认模型、序列长度等。如果你愿意，可以忽略大多数（如果不是全部）这些设置。
3. 启动 TabbyAPI。如果成功，你应该会看到类似这样的内容：

![TabbyAPI terminal](/static/tabby-terminal.png)

3. 在 SillyTavern 的文本完成 API 下，选择 TabbyAPI。
4. 将你的 API 密钥从 TabbyAPI 终端复制到 `Tabby API key` 中，并确保你的 `API URL` 是正确的（默认应该是 `http://127.0.0.1:5000`）。

如果你一切都做对了，你应该会在 SillyTavern 中看到类似这样的内容：

![TabbyAPI SillyTavern](/static/tabby-config.png)

你现在可以使用 TabbyAPI 聊天了！

### TabbyAPI 加载器
TabbyAPI 的开发者创建了一个官方扩展，可以直接从 SillyTavern 加载/卸载模型。安装步骤简单：
1. 在 SillyTavern 中，点击扩展选项卡并导航到下载扩展和资产。
2. 将 `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` 复制到资产 URL 中，并点击右侧的插头按钮。
3. 你应该会看到类似的界面。点击 Tabby Loader 旁边的下载按钮。

![Tabby Loader](/static/tabby-assets.png)

4. 如果安装成功，你应该会在屏幕顶部看到一个绿色的弹出消息。在扩展选项卡下，导航到 TabbyAPI Loader，并将你的管理员密钥从 TabbyAPI 终端复制到管理员密钥中。
5. 点击模型选择旁边的刷新按钮。当你点击下面的文本框时，你应该会看到模型目录中的所有模型。

![Tabby Loader Extension](/static/tabby-loader.png)

你现在可以直接从 SillyTavern 加载和卸载模型！

### 支持
仍然需要帮助吗？访问 [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) 获取开发者官方 Discord 服务器的链接，并 [阅读维基](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started)。