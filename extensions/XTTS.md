# XTTS 与声音克隆

你好！所以，你被那些展示了人工智能文本转语音技术进展的 Reddit 帖子震惊了吗？

想给你的机器人老婆/老公一个新的闪亮的声音调制器吗？

不用担心，这项令人惊叹的突破性技术已经在你当地的 SillyTavern 可用，你只需要一个简单的... 

```
<code block content>
```

## 前提条件

1. 最新的 `staging` 分支的 SillyTavern。
2. 已安装 [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html)。
3. (Windows) 已安装 [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)。
4. 包含语音剪辑的 WAV 文件（每个文件约 10 秒）。文件要求：PCM，单声道，22050Hz，16 位（通过 Audacity 转换）。
5. 创建一个包含 "speakers" 和 "output" 子文件夹的文件夹。将 WAV 文件放入 "speakers" 中。

示例文件夹结构：
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## 安装 

[daswer123](https://github.com/daswer123) 制作了一个 API 服务器，可以在您的计算机上运行 XTTSv2 模型，并连接到 SillyTavern 的 TTS 扩展。

它与 Extras API 完全独立，并将使用一个单独的环境。

**非常重要：** 请不要将以下要求安装到您的 Extras 环境或系统 Python 中。
这会破坏您的其他包，导致不必要的降级等。

以下说明使用 Miniconda 提供，但您也可以使用 venv（此处不予覆盖）。
打开 Anaconda 命令提示符，并逐行按照说明操作。

### 启动服务器

1. 导航到您在先决条件的第4步中创建的文件夹。
```
cd C:\xtts
```
2. 创建一个新的 conda 环境。从现在开始，我们称之为 `xtts`。
```
conda create -n xtts
```
3. 激活新创建的环境。
```
conda activate xtts
```
4. 在您的环境中安装 Python 3.10。出现提示时确认输入 "y"。
```
conda install python=3.10
```
5. 安装 XTTS 服务器及其依赖项。
```
pip install xtts-api-server pydub
```
6. 安装 PyTorch。这可能需要一些时间。以下命令安装支持 GPU 加速 (CUDA) 的 PyTorch。
如果您只想使用 CPU 推理，请去掉以 `--index-url` 开头的最后部分。
```
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```
7. 在默认主机和端口启动 XTTS 服务器：<http://localhost:8020>
```
python -m xtts_api_server
```
8. 在您的第一次启动期间，模型将被下载（约 ~2 GB）。
不要忘记仔细阅读 Coqui AI 的法律声明。哈哈，我在开玩笑，只需再次输入 "y"。

### 连接到 SillyTavern

1. 打开扩展面板，展开 TTS 菜单，并在提供者列表中选择 "XTTSv2"。
2. 在语言下拉菜单中选择您的文本转语音语言（如果不是波兰语我会很难过）。
3. 验证提供者端点指向 <http://localhost:8020>，并且“可用声音”显示您的声音样本列表。
4. 选择任何角色，并在声音样本与角色之间设置映射。
如果角色列表为空，请点击“重新加载”几次。
5. 根据您的偏好配置其余的 TTS 设置。

### 现在一切准备就绪！

在任何消息的上下文操作菜单中点击扬声器图标，聆听从扬声器中传出的美妙克隆声音。生成需要一些时间，即使在高端RTX GPU上也不是实时的。

### 流媒体？

可以使用最新版本的 XTTS 服务器通过 HTTP 流媒体获取生成的音频块，一旦可用就可以获取！

#### 这在 RVC 中不起作用！

音频仍然会被生成（假设您使用的是最新版本的 RVC 扩展）并进行转换，*但不会进行流媒体传输*，因为 RVC 需要在启动转换之前拥有完整的音频文件。流媒体 RVC 仍在研究中...

#### 如何获得流媒体支持？

1. 更新到 SillyTavern 的最新 `staging` 分支。
2. 将 XTTS 服务器更新到最新版本。
```
conda activate xtts
pip install xtts-api-server --upgrade
```
3. 像往常一样启动并连接 XTTS 到 ST。
4. 在 SillyTavern 中启用 "Streaming" XTTS 扩展设置。

### 音频卡顿？

尝试增加“块大小”设置。

作为参考：在块大小为200的情况下，RTX 3090可以以稍微增加的音频延迟产生不间断的音频。

### 如何重启 TTS 服务器？

只需执行安装说明中的第 1、3 和 7 步。

### Android??

不太可能，它无法运行需要 PyTorch 的应用程序，除非使用某些我们不提供支持的神秘黑魔法。您可以在自己的风险下尝试，但如果遇到任何问题将不提供支持。

您最好的解决方案是在本地网络上通过您的 PC 托管 TTS API，只需记得指定要监听的主机和端口 - 请参见 [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md).