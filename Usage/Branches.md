---
icon: git-branch
---

## 分支

SillyTavern 正在使用多分支系统进行开发，以确保所有用户的顺畅体验。

* **release** - 推荐给大多数用户。这是最稳定和推荐的分支，仅在发布重大版本时更新。适合大多数用户。
* **staging** - 不推荐随意使用。此分支具有最新功能，但请小心，因为它可能随时中断。仅适合高级用户和爱好者。
* **main** 和 **dev** - 这些分支已被弃用，将不再接收更新，并将在 *2023年9月1日* 删除。建议您在截止日期之前切换分支。

## 主分支和开发分支发生了什么？

git 提交历史被压缩到 1.9.0 版本的状态。

旧分支承载着长期存在的不良 git 实践和曾经存在于仓库中的大型二进制文件。
每次 git clone 都会下载并存储大量不必要的文件，造成网络和磁盘负担。
此外，这使得我们几乎不可能在不引发 git 冲突的情况下，正确更新默认内容，例如 settings.json。

不幸的是，失去提交历史也意味着失去代码贡献历史。
如果您对 SillyTavern 的开发做出了贡献，并希望在 README 文件和文档网站上看到您的名字，请与我们联系！

## 如何在使用 main/dev 的情况下迁移到新分支？

_**建议进行全新安装。**_ 但是，如果您希望使用现有的 SillyTavern 副本，请按照以下说明进行操作。

**重要！** 在进行任何操作之前，请对您的安装做 *完整备份*。您可能会 *丢失数据*，所以不要忽视这个警告。

不确定要备份哪些文件？请参见此处的列表：[如何更新 SillyTavern](https://docs.sillytavern.app/usage/update/#note-do-not-copy-the-entire-public-folder)

### git 安装

1. 在您的 SillyTavern 安装文件夹中打开终端提示符 (cmd, PowerShell, Termux 等)。
2. 输入 `git fetch` 然后输入 `git pull` 以拉取更新。
3. 您可能会丢失设置。您是否做了备份？`git switch release` 或 `git switch staging` 将分别更改您的分支。
4. 如果没有错误，请跳到下一项。您可能会看到类似以下内容：
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   您将看到受影响文件的列表。如果您不在乎这些设置文件被替换，`git switch -f release` 或 `git switch -f staging` 将设置您的分支。
   如果您想保留这些更改，请从备份中恢复。
7. 输入 `npm install` 然后输入 `npm run start` 以测试一切是否正常。
8. 享受吧！如有需要，请从备份中恢复您的数据。

### fatal: invalid reference: release

如果您仅从旧的远程（在迁移到组织仓库之前）克隆了一个分支，这种情况可能会发生。要解决此问题，您需要从新的远程添加并获取一个分支：

```
git remote add st http://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

然后从第5步继续。

### ZIP 安装

对您来说没有任何变化。只需像往常一样下载分支/发布的 ZIP 文件。