---
label: 常见问题
icon: bug
---

# 附加组件安装常见问题

本页面列出了在安装 SillyTavern 附加组件时遇到的常见问题和疑问。

---
在您的操作系统上（尤其是 Termux）本地安装附加组件可能会很困难或不可能。

#### 使用 [官方附加组件 Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* 设置简单
* 免费使用
* 无需 Colab GPU 额度（使用 `use_cpu` 选项）
* 有关详细信息，请参阅 [Colab 指南页面](https://docs.sillytavern.app/extras/running-extras-in-colab/)。

---

### 错误：无法在 Linux 上导入 'talkinghead' 模块

由于与 Colab 不兼容，它需要安装一个额外的软件包，因为它不会自动安装。在安装其他依赖项后运行以下命令：

`pip install wxpython`

### 附加服务器无法连接到 AUTOMATIC1111 的 Stable Diffusion Web UI

> 无法连接到远程 SD 后端 <http://127.0.0.1:7860>！正在禁用 SD 模块...

**确保启动 Stable Diffusion 的 webui-user.bat 文件包含 --api 命令行选项在 COMMANDLINE_ARGS 变量中。**

在你的 "webui-user.bat" 中找到并替换该行： `set COMMANDLINE_ARGS=--api`

![它应该是这样的](/static/extensions/sd-user.png)

如果 SD Web UI 的 API 模式被禁用，附加服务器将无法建立连接，你将无法生成图像！

#### 仍然无法工作？

确保你按照正确的顺序启动所有程序，等待每个程序加载完成后再进行下一步：

1. Stable Diffusion Web UI
2. SillyTavern 附加功能
3. SillyTavern

如果附加服务器在 Stable Diffusion API 之后加载，则无法重新连接。

### hnswlib 构建轮子时安装 ChromaDB 的错误

> 错误：无法为 hnswlib 构建轮子，这对于安装基于 pyproject.toml 的项目是必需的

在安装 ChromaDB 模块之前，您必须首先执行 `以下操作之一`：

* 安装 Visual C++ 构建工具：<https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* 使用 conda 安装 `hnswlib` 包：`conda install -c conda-forge hnswlib`

---

### 在Mac上安装Python依赖时出错

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac不支持CUDA，因此torch包应在没有CUDA支持的情况下安装。

请使用`requirements-silicon.txt`文件安装依赖。

### 缺少模块？

* 您必须在 Python 命令行中指定模块名称列表，并使用 `--enable-modules` 修饰符。
* 请参阅 [Modules](https://docs.sillytavern.app/extras/installation/#decide-which-module-to-use) 部分。

---

### API Key框的用途是什么？

* SillyTavern的扩展面板中的API Key框仅在您：
  * 在您的Extras安装文件夹中创建了一个名为`api_key.txt`的文本文件，该文件包含您选择的Extras '密码'。
  * 使用`--secure`命令行参数启动了扩展。
* 这使得Extras API '密码锁定'，因此只有在其API Key框中拥有该密钥的用户才能访问它。
* 这主要对想要自己公开部署Extras（colab等）的人有用。
* 在个人电脑上运行Extras以供个人使用的用户不应在API Key框中输入任何内容。

### 移动设备/Android/Termux 怎么样？🤔

* 社区中有一些人成功地通过 Termux 在手机上运行 Extras。
* 然而，Extras 并不是为了移动设备支持而设计的。
* 不会为在 Android 设备上运行 Extras 的用户提供支持。
* 请将所有问题直接指向下面链接的指南创建者。

#### ❗ 这是不受支持的

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>