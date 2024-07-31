# 实时语音克隆 (RVC)

本指南将引导您使用 RVC，这是一种允许将语音特征从一个音频片段转移到另一个音频片段的技术，使声音能够以不同的语调和风格进行讲话。

您是否曾享受过那些著名的“总统玩 X”视频？它们是使用 RVC 创建的。借助 RVC Extra 模块，您可以让您的 SillyTavern 角色以您希望的任何声音进行对话，无论是动漫、电影，还是您自己独特的声音。

RVC 不是 TTS：它更像是语音到语音。它将音频片段作为输入。在后台，RVC 与 SillyTavern-extras 的 TTS 模块协同工作：它等待 TTS 生成音频文件（无论您是否使用 RVC，TTS 都会执行此操作），然后 RVC 将执行第二次处理，将 TTS 音频文件转换为您 RVC 配置中的克隆声音。

## RVC 设置

### 先决条件

在开始之前，请确保您满足以下先决条件：

- 从扩展面板（堆叠块图标）中的“下载扩展和资产”菜单安装“RVC”扩展。
- 确保您的 PATH 环境变量中包含 `ffmpeg` 可执行文件。此工具用于转换传入的音频。
   - 使用 Launcher.bat 脚本中的工具箱自动安装 ffmpeg: https://github.com/SillyTavern/SillyTavern-Launcher/blob/main/Launcher.bat
   - 或在此处下载构建版本: https://www.gyan.dev/ffmpeg/builds/ 
   - 如何修改 PATH 变量: https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/
   - 要测试您是否正确操作，请打开命令提示符并运行 ```ffmpeg```。它应该打印 ffmpeg 版本和信息。

### 步骤设置

1. **准备 RVC 模型文件**:
   - 在文件浏览器中，导航到: `\SillyTavern-extras\data\models\rvc` 
   - 创建一个名为 'Betty' 的子文件夹，并将 `.pth` 和 `.index` 文件放入其中。（提示：您可以从 https://voice-models.com 下载语音文件，请确保语音名称显示为 RVPME。）

2. **安装依赖**:
   - 使用命令安装必要的依赖: `pip install -r requirements-rvc.txt`。

3. **确保 TTS 已启用并正常工作**
   - RVC 依赖 TTS，您需要启用 TTS 模块。请参阅我们的 TTS 指南以了解如何操作。在尝试将 RVC 添加到混合中之前，您的 TTS 必须已经正常工作并能够朗读您的聊天内容！

4. **运行启用 RVC 的 SillyTavern-extras**:
   - 启动启用 RVC 模块的 SillyTavern-extras。这个示例调用假设您使用的是 Silero TTS，它随 SillyTavern-extras 一起预装：
     ```shell
     python server.py --enable-modules=rvc,silero-tts
     ```
     可选地，如果您有一个强大的 GPU，您可以通过在启动命令中添加 ```--cuda``` 来运行 RVC。根据快速测试，朗读 50 个标记（约 36 个单词）时 VRAM 使用量为 3.4GB，朗读 200 个标记（约 150 个单词）时为 7.6GB。

5. **在 SillyTavern 中配置 RVC**:
   - 在 SillyTavern 中，导航到 **扩展** > **RVC** 并启用它。

6. **设置语音映射**:
   - 为 RVC 创建一个语音映射。将您的角色设置为您希望的 SillyTavern 角色名称，并将语音设置为您在第 1 步中创建的 RVC 文件夹，然后点击应用。如果您操作正确，语音映射将显示类似 'Betty:MyVoice(rvpme)' 的内容。

7. **选择音高提取**:
   - 选择 "rmvpe" 作为音高提取方法。
   - 如果您在使用 "rmvpe" 时遇到问题，请尝试其他方法。

8. **（可选）配置 RVC 将您的生成保存到文件**:
   - 如果出于测试或故障排除的目的，您希望保存生成的 RVC 音频，请在启动命令中添加 ```--rvc-save-file```。这将把最后生成的文件保存到 SillyTavern-extras\data\tmp\rvc_output.wav：
   ```shell
   python server.py --enable-modules=rvc,silero-tts --rvc-save-file
   ```

### 基于表达的动态语音

1. **配置 RVC 模型**:
   - 在您的 RVC 模型文件夹中，为每个分类的表达（例如，愤怒、恐惧、快乐、爱、悲伤、惊讶）准备单独的 `.pth` 和 `.index` 文件。

2. **启用模块**:
   - 启用 RVC 和分类模块：
     ```shell
     python server.py --enable-modules=rvc,classify
     ```

3. **使用 RVC 模块**:
   - 剩余的设置与单独使用 RVC 模块类似（如上所述）。

## 训练您自己的 RVC 模型

### 使用 Deffcolony 的 RVC 简易菜单（仅限 Windows）

自动安装并启动 Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

1. **克隆仓库**:
   - 将仓库克隆到您希望的位置:
     ```shell
     git clone https://github.com/deffcolony/rvc-easy-menu.git
     ```

2. **启动 RVC-Launcher.bat**:
   - 打开 `RVC-Launcher.bat` 文件。
   - 选择选项 1 安装 RVC。

3. **完成安装**:
   - 当提示时，安装所需的包和依赖项。

4. **打开用于语音训练的 WebUI**:
   - 安装完成后，选择选项 2 打开用于语音训练的 WebUI。

### Mangio-RVC: 训练语音模型

**数据集准备**：

1. **准备音频**：
   - 将要训练的音频放入 `datasets` 文件夹。
   - 确保音频没有背景噪音 – 只需要纯粹的语音。
   - 较长的音频能够提供更好的输出质量。

**WebUI 训练**：

1. **访问训练标签**：
   - 点击 WebUI 中的训练标签。

2. **配置实验**：
   - 输入实验名称（例如，`my-epic-voice-model`）。
   - 将版本设置为 v2。

3. **处理数据并提取特征**：
   - 点击“处理数据”和“特征提取”。
   - 将“保存频率”设置为 50。

4. **训练参数**：
   - 将“总训练轮数”设置为 300。
   - 点击“训练特征索引”和“训练模型”。