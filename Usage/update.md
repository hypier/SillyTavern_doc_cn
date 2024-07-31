---
order: 99
icon: repo-pull
---

# 如何更新 SillyTavern

在下面找到您的操作系统，并按照说明更新 ST。

（本指南假设您已安装过 SillyTavern，并知道如何在您的操作系统上运行它。）

这不是安装指南。如果您需要安装说明，请访问您操作系统的安装页面（可以在网站导航中找到）。

## Linux/Termux

您肯定是通过 git 安装的，因此只需在 SillyTavern 目录中执行 'git pull'。

- `cd SillyTavern` 以进入正确的文件夹。
- `git pull` 以获取更新。
- `./start.sh` 或 `bash start.sh` 启动 ST。

----

## Windows/MacOS

>首先尝试使用位于您的 SillyTavern 安装基础文件夹中的 `UpdateAndStart.bat`。

如果失败，请返回这里继续阅读。

### 方法 1 - GIT

我们始终建议用户使用 'git' 安装。原因如下：

当您通过 `git clone` 安装后，更新只需在 ST 文件夹的命令行中输入 `git pull` [在命令行中](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder)。
另外，如果命令提示符给您带来问题（并且您已安装 GitHub Desktop），您可以使用 `Repository` 菜单并选择 `Pull`。

更新会自动且安全地应用。

#### “帮助，我最初是通过 Zip 安装的，现在想转换为 Git 安装”

您选择了一条明智的道路。

由于您的安装是通过 Zip 完成的，您需要使用 git 进行新的安装。

幸运的是，我们有 [说明](https://docs.sillytavern.app/installation/windows/) 来指导您如何操作。

一旦您使用 git 在不同的文件夹中安装了一个新的 SillyTavern，请返回此页面并继续进行下面的 'Zip 更新' 说明中的 **步骤 4**。

### 方法 2 - ZIP

如果您坚持通过 ZIP 安装，这里是进行更新的繁琐过程：

1. 下载新的发布 ZIP。
2. 将其解压到您当前 ST 安装之外的一个文件夹中。
3. 根据您的操作系统执行常规设置程序以安装 NodeJS 需求。

4. 根据需要从您旧的 ST 安装中复制以下文件/文件夹(*)：

(*) “根据需要” = “如果您对那些文件夹做了任何自定义内容”。

#### 更新 >=1.12.0

从一个安装复制 `/data` 目录和 `config.yaml` 文件到另一个安装。

#### 从 <1.12.0 更新到 >1.12.0

1.12.0 包含自动迁移程序。以下步骤仅在迁移被中断或出错时需要执行。

5. 至少运行更新后的服务器安装一次，以创建 `/data/default-user` 目录。
6. 根据需要将旧的 `/public` 中的文件转移到新的 `/data/default-user`。

没有任何文件夹是强制性的，因此只复制您需要的内容。

**注意：请勿复制整个 /PUBLIC/ 文件夹**

这样做可能会破坏新的安装并导致新功能无法使用。

```plaintext
Assets
Backgrounds
Characters
Chats
Context
Groups
Group chats
Instruct
movingUI
KoboldAI Settings
NovelAI Settings
OpenAI Settings
QuickReplies
TextGen Settings (textgen = ooba)
Themes
User Avatars
Worlds
User
settings.json
secrets.json <---- 这个在基础文件夹中，而不是 /public/
```

7. 一旦那些文件夹/文件被复制，粘贴到新安装的 /data/default-user 文件夹中（secrets.json 放入文件夹根目录）。
8. 根据您的操作系统再次启动 SillyTavern，并祈祷您做对了。
9. 如果一切正常显示，您可以安全地删除旧的 ST 文件夹。

### 常见更新问题
#### “工作目录中存在未解决的冲突。”

这意味着您修改了在远程仓库中被更改的默认文件（例如设置预设）。

要解决此问题，请在终端中运行以下命令。请谨慎使用，因为这可能会造成破坏。如有需要，请确保备份。

```
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### 文件更改阻止 git pull

- 如果您更改了 SillyTavern 系统文件，`git pull` 可能无法正常工作。
- 有时更新可能要求我们更改一个重要文件，这可能导致同样的问题。
- 通常是默认预设文件或 `package-lock.json`。
- 在这种情况下，您可以尝试将文件移动到其他文件夹（或删除该文件），然后再执行 `git pull`。
- 另一种解决方案是使用 `git pull --rebase --autostash`

#### 错误：启动服务器时无法找到模块 "***"

- 这意味着 SillyTavern 添加了新的 npm 包要求。
- 在 SillyTavern 目录中运行 `npm install` 来解决此问题。提供的 Start.bat 和 start.sh 脚本会自动执行此操作。
- 仍然没有帮助？删除 node_modules 文件夹

**Windows**
```
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**
```
rm -rf node_modules
npm cache clean --force
npm install
```