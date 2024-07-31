---
label: 通过 Colab 的附加功能
order: 100
icon: cloud
---

# 通过 Colab 运行额外功能

---

## Extras项目自2024年4月起已停止维护，将不再接收任何新的更新或模块。绝大多数模块已在主SillyTavern应用程序中本地提供。您仍然可以使用Colab笔记本，但如果遇到任何问题，请不要指望能立即获得支持。
---

运行SillyTavern Extras Colab的说明。

* 打开[官方Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* 选择所需的“额外”选项
* 选择`use_cpu`以在不需要GPU额度的情况下运行Extras
  * 这会使Stable Diffusion变慢，但其他一切将正常运行
* 不强制要求，但推荐：选择`secure`选项以生成API密钥以保护您的共享实例。
* 点击左侧的开始按钮（看起来像一个三角形的“播放”按钮）
* 等待加载完成
* 在输出底部查找`trycloudflare.com`链接。忽略localhost链接，它将无法工作（我们试过了！）。
* 它将以文本`Running on`开头
* 复制该行下方列出的API URL链接。（**请勿复制“localhost” URL，使用其他链接**）
* 启动支持扩展的SillyTavern：（如有必要，将`enableExtensions`设置为`true`在您的`config.yaml`中）
* 导航到SillyTavern的扩展菜单（点击页面顶部的“叠块”图标）。
* 将API URL粘贴到顶部的框中。（**不是API密钥框**）
* 如果您没有启用`secure`选项，请确保在使用官方colab时API密钥框完全为空。
* 如果您启用了`secure`选项，请将生成的API密钥粘贴到API密钥框中。
* API密钥将出现在colab的控制台输出中，例如：`Your API key is fee2f3f559`
* 点击“连接”