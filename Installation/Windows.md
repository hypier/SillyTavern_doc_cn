---
order: 10
label: Windows
---

# Windows 安装

!!!warning
请勿安装到任何 Windows 控制的文件夹（Program Files, System32 等）。

请勿以管理员权限运行 START.BAT

在 Windows 7 上安装是不可能的，因为它无法运行 Node.js 18.16
!!!

## 通过 Git 安装
  1. 安装 [NodeJS](https://nodejs.org/en)（推荐最新的 LTS 版本）
  2. 安装 [Git for Windows](https://gitforwindows.org/)
  3. 打开 Windows 资源管理器 (`Win+E`)
  4. 浏览到或创建一个不受 Windows 控制或监视的文件夹。（例如：C:\MySpecialFolder\）
  5. 在该文件夹内打开命令提示符，点击顶部的“地址栏”，输入 `cmd`，然后按 Enter。
  6. 当黑色窗口（命令提示符）弹出时，输入以下其中之一并按 Enter：

- 对于 Release 分支： `git clone https://github.com/SillyTavern/SillyTavern -b release`
- 对于 Staging 分支： `git clone https://github.com/SillyTavern/SillyTavern -b staging`

  7. 一旦所有内容克隆完成，双击 `Start.bat` 以使 NodeJS 安装其依赖项。
  8. 服务器将启动，SillyTavern 将在您的浏览器中弹出。

## 通过 SillyTavern 启动器安装
1. 在键盘上：按 **`WINDOWS + R`** 打开运行对话框。然后，运行以下命令以安装 git：
```shell
cmd /c winget install -e --id Git.Git
```
2. 在键盘上：按 **`WINDOWS + E`** 打开文件资源管理器，然后导航到您想要安装启动器的文件夹。一旦进入目标文件夹，在地址栏中输入 `cmd` 并按回车。然后，运行以下命令：
```shell
git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
```

## 通过 GitHub Desktop 安装
（这仅允许在 GitHub Desktop 中使用 git，如果您还想在命令行中使用 `git`，您还需要安装 [Git for Windows](https://gitforwindows.org/)）
  1. 安装 [NodeJS](https://nodejs.org/en)（推荐最新的 LTS 版本）
  2. 安装 [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
  3. 安装 GitHub Desktop 后，点击 `从互联网克隆一个代码库....` （注意：您**不需要**为此步骤创建 GitHub 账户）

  ![image](/static/windows-1.png)

  4. 在菜单中，点击 URL 标签，输入此 URL `https://github.com/SillyTavern/SillyTavern`，然后点击克隆。您可以更改本地路径以更改 SillyTavern 的下载位置。

  ![image](/static/windows-2.png)

  6. 要打开 SillyTavern，请使用 Windows 资源管理器浏览到您克隆代码库的文件夹。默认情况下，代码库将克隆到这里：`C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`
  
  7. 双击 `start.bat` 文件。（注意：文件名中的 `.bat` 部分可能会被您的操作系统隐藏，在这种情况下，它看起来像一个名为 "`Start`" 的文件。这就是您双击以运行 SillyTavern 的文件）

  ![image](/static/windows-3.png)

  8. 双击后，应该会打开一个大的黑色命令控制台窗口，SillyTavern 将开始安装其所需的操作环境。
  
  9. 安装过程结束后，如果一切正常，命令控制台窗口应该像这样，并且您的浏览器中应该打开一个 SillyTavern 标签：

  ![image](/static/windows-4.png)

  10. 连接到任何 [支持的 API](https://docs.sillytavern.app/usage/api-connections/) 并开始聊天！