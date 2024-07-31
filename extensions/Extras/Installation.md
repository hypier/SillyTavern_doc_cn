---
icon: gear
label: 本地安装
---

# 附加组件安装

本页面包含在您的本地设备上安装 SillyTavern 附加组件的说明。

---

## Extras项目自2024年4月起已停止维护，不会再接收任何新的更新或模块。绝大多数模块在主SillyTavern应用中已原生提供。您仍然可以安装并使用它，但如果遇到任何问题，请不要指望能得到即时支持。
---

在您的操作系统上（尤其是Termux），本地安装Extras可能会很困难或不可能。

#### 使用[官方Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* 简单设置
* 免费使用
* 无需Colab GPU积分（使用`use_cpu`选项）
* 有关详细信息，请参见[Colab指南页面](https://docs.sillytavern.app/extras/running-extras-in-colab/)。

---

## 安装方法

### MiniConda（推荐）

此方法推荐使用，因为Conda为Extras所需的包创建了一个“虚拟环境”，这样它们就不会影响您系统范围内的Python设置。

1. 安装 [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

_(重要！) 阅读 [如何使用Conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. 安装 [git](https://git-scm.com/downloads)

_(最初通过git安装SillyTavern的Chads可以跳过此步骤！)_

安装完这两个之后...

在 `CONDA COMMAND PROMPT WINDOW` 中逐个输入/粘贴以下命令，并在每个命令后按 `Enter`。

3. 创建一个新的Conda环境（我们称之为 `extras`）：

`conda create -n extras`

4. 激活新环境

`conda activate extras`（您应该在命令提示符的左侧看到 `(extras)`）

5. 安装所需的系统包（这将花费一些时间）

`conda install python=3.11 git`

6. 克隆Extras GitHub仓库

`git clone https://github.com/SillyTavern/SillyTavern-extras`

7. 导航到您克隆的Extras仓库

`cd SillyTavern-extras`

8. 使用以下命令之一安装Extras的依赖项（这也需要一些时间）：

* `pip install -r requirements.txt` - 用于基本功能
* `pip install -r requirements-rvc.txt` - 用于实时语音克隆
* `pip install -r requirements-coqui.txt` - 用于Coqui TTS（不推荐）

如果在此步骤中遇到错误，请参阅 [常见问题](https://docs.sillytavern.app/extras/installation/common-problems/) 页面！

9. 请参阅下方“安装后运行Extras”

### 系统范围安装

这更简单，但会影响您的系统范围的 Python 安装。

如果您使用许多具有不同要求的 Python 程序，这可能会导致冲突。

如果这是您第一次接触与 Python 相关的内容，那应该不会成为问题。

1. 安装 Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. 安装 git: <https://git-scm.com/downloads>
3. 打开命令提示符窗口，并进入您具有完全访问权限的文件夹。
4. 克隆仓库: `git clone https://github.com/SillyTavern/SillyTavern-extras`，按 Enter。
5. 克隆完成后，输入 `cd SillyTavern-extras`，按 Enter。
6. 输入 `python -m pip install -r requirements.txt`
7. 请参见下面的“安装后运行附加功能”

## 安装后运行附加程序

### 确认扩展已启用

1. 在文本编辑器中打开名为 `config.yaml` 的文件。该文件位于 ST 的基本安装文件夹中。
2. 查找包含 `enableExtensions` 的行。
3. 确保该行的值为 `true`，而不是 `false`。

### 决定使用哪个模块

（这只需要做一次）

* Extras 始终通过 Python 命令行启动。
* `python server.py` 是最低限度，但不会启用任何有用的模块。
* 要启用模块，必须使用 `--enable-modules=` 修饰符，并提供以逗号分隔的模块名称列表。

示例：`python server.py --enable-modules=caption,summarize,classify`

这将启用图像字幕、聊天摘要和实时更新的角色表情。

下面是描述每个模块的表格。

| 名称        | 描述                               |
| ----------- | ---------------------------------- |
| `caption`   | 图像字幕                           |
| `summarize` | 文本摘要                           |
| `classify`  | 文本情感分类                       |
| `sd`        | 稳定扩散图像生成                   |
| `silero-tts`| [Silero TTS 服务器](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`  | [Microsoft Edge TTS 客户端](https://github.com/rany2/edge-tts) |
| `chromadb`  | 向量存储服务器                     |
| `coqui-tts` | Coqui TTS                         |
| `rvc`       | 实时语音克隆                       |

* 决定要添加到 Python 命令行中的模块。
* 它们将在下一步中使用。

**注意：您的 Python 命令模块列表中绝对不能有空格！**

### 启动 Extras 服务器

在 Extras 安装文件夹内的命令提示符窗口中...

1. 确保您的 conda 环境处于活动状态（如果您使用了 Conda 安装方法）
2. 如果环境未激活，请输入 `activate extras`。
3. 输入 `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. extras 服务器将加载。
5. 过一会儿，它会在最后显示一个 URL。对于本地安装，默认为 `http://localhost:5100`。
6. 复制 API URL。

### 将 ST 连接到 Extras 服务器

1. 启动您的 SillyTavern 服务器，并在浏览器中查看 SillyTavern 界面。
2. 打开扩展面板（通过页面顶部的“叠块”图标）
3. 将 API URL 粘贴到输入框中。
4. 点击 `Connect`。

要再次运行 Extras，只需激活环境并在命令提示符中运行以下命令。

`conda activate extras`，按 Enter。
`python server.py`，按 Enter。

确保您设置所需的 server.py 的其他选项（见下文）。

## 创建一个 .bat 文件以便于启动

这一步是可选的，仅适用于 Windows，但在 MacOS 上应该也可以做到类似的操作。

1. 查看您的 Windows 桌面
2. 右键单击，选择 `新建`，然后点击 `文本文件`
3. 桌面上会出现一个新文件，要求输入名称。
4. 将文件命名为 `STExtras.txt`
4. 在文本编辑器中打开新创建的文件。
5. 将以下代码粘贴到其中：

```
cd C:\_your_\_full_\_Extras_\_folder_\_path_\
call conda activate extras
python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
call conda deactivate
pause
```

6. 用您实际的 Extras 安装文件夹路径替换占位符文件夹路径。
7. 用您实际的命令行替换 python 命令行
8. 将文件另存为新名称 `STExtras.bat`（在大多数文本编辑器中使用 `文件` >> `另存为`）

现在您可以简单地双击这个 .bat 文件以便于启动 Extras。

如果您想更改模块列表（或任何其他 extras 服务器的命令行修饰符），只需编辑 .bat 文件中的 python 命令即可。