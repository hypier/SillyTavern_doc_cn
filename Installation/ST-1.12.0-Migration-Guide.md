# SillyTavern 1.12.0 迁移指南

SillyTavern 1.12.0（代号“新服务器”更新）包含几个关键更改，这些更改可能会影响您使用 SillyTavern 的方式。

本指南将帮助您为更新做好准备，并提供进一步的指导。

## 数据存储更新

1.12.0 更改了 SillyTavern 处理用户数据的方式。

之前，所有持久数据与前端部分一起存储在 `/public` 目录中，这造成了混淆和潜在的故障点，并且使容器化和系统范围的应用安装变得相当具有挑战性。

### 有什么变化？

所有来自 `/public` 的持久信息，如设置和聊天（完整列表如下）已移动到一个具有可配置路径的单独目录，使其可移植且与网络服务器本身独立。当出于兼容性目的需要时，例如用于托管扩展、全尺寸角色卡、用户图像上传等，已设置智能重定向以自动从数据目录托管用户文件。

### 设置数据根目录

您可以通过 `config.yaml` 提供绝对路径或相对路径（相对于 ST 存储库目录）来设置数据根目录，或者通过使用 `--dataRoot` 控制台参数启动服务器。

> YAML 示例

```yaml
# -- 数据配置 --
# 用户数据存储的根目录
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> 控制台示例

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# 或者
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

默认的数据根路径是 `./data`，这意味着 SillyTavern 存储库中的 `data` 目录。

!!! info 注意
数据根路径应该是 **完整的绝对路径** 或 **完整的相对路径**。您 _不能_ 使用像 `~` 或 `%APP_DATA%` 这样的路径快捷方式，因为这些是由 shell 解析的，而不是操作系统。

### 迁移

#### **重要！** 在我们开始之前

1. **仅在您想要将 dataRoot 从默认位置移动时使用。否则，请跳过此部分。** 在首次运行服务器并更新后，_在此之前_ 设置数据根。运行 `npm install` 以使 `config.yaml` 填充新值，或传递控制台参数。
2. 所有数据将迁移到 `default-user` 账户中。有关更多信息，请参见下面的 [用户](#users)。

#### 无容器（裸金属）安装

您无需做任何事情！当您启动 ST 服务器并检测到旧存储格式（通过检查 `/public/characters` 目录的存在）时，自动迁移将为您处理所有内容。

在移动任何文件时，将在 `/backups/_migration/YYYY-MM-DD`（解析为当前日期）目录中创建自动备份，但在运行迁移之前，始终是一个好习惯进行完整的手动备份。

#### 容器化（Docker）安装

在 Docker 卷中迁移数据稍微复杂一些，但非常简单。虽然与仓库一起提供的 `docker-compose.yml` 已更新以反映更改，但您可能需要调整自定义工作流程/部署。

**第 1 步。** 创建一个新卷，并将其挂载到容器内的 "/home/node/app/data" 路径。不要删除 `config` 卷。

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**第 2 步。** 将除 `config.yaml` 文件外的所有内容从 `config` 卷移动到 `data` 卷的 `default-user` 子目录中。

**第 3 步。** 重新构建容器并启动它。

!!! info 注意
`/public` 目录与 `config` 卷之间的软链接不再需要，并且未构建到 Docker 容器中！
!!!

#### 要迁移什么？

以下文件和目录将进行数据迁移。假设默认配置，下面的表格提供了迁移前后的路径。

| 迁移前                                 | 迁移后                                |
| -------------------------------------- | ------------------------------------ |
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## 用户

1.12.0 增加了一个（完全可选的）功能，可以在同一服务器上创建多用户设置，允许多个用户同时使用各自完全隔离的 SillyTavern 实例。用户账户还可以设置密码保护，以增加隐私层级。

> 尽管我们尝试应用大多数知名的安全实践，但这仍然不意味着 SillyTavern 服务器足够安全以暴露于公共互联网。

!!! danger 免责声明
**在确保适当的安全措施之前，绝不要将任何实例托管到开放互联网。**

**对于因不当或不充分的安全实施导致的未经授权访问造成的任何损害或损失，我们不承担责任。**
!!!

### 配置

要启用和使用多用户模式，请编辑 `config.yaml` 文件：

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. 当用户账户设置被禁用时，将使用 `default-user` 备用管理员账户来存储用户数据。
2. 当隐秘登录设置被禁用时，登录屏幕上会显示活跃用户列表。如果启用，用户必须手动输入他们的用户名。

> 你不能 _删除_ 用户列表中的 `default-user` 账户，因为它用于在 `enableUserAccounts` 设置为 `false` 时提供用户数据。但你可以 _禁用_ 它，以将其隐藏在列表中并禁止登录。

### 用户标识

用户标识是用户的唯一标识符。它只能由小写字母、数字和短横线组成。

用户数据目录的路径假设使用以下模式：`%DATA_ROOT%/%USER_HANDLE%`。

有效用户标识的示例：

-   default-user
-   juan555
-   flux-the-cat
-   cool-guy1337

### 用户角色

-   **管理员** - 可以管理（创建、删除、修改）其他用户。
-   **用户** - 无法管理其他用户。

除了拥有管理员面板访问权限外，两个用户角色在功能上是相同的，并且可以在没有任何限制的情况下使用SillyTavern的所有功能。用户权限的实现待定。

所有用户账户最初都创建为普通用户，必要时可以提升为管理员。

### 登录界面

您可以选择一个用户账户进行使用。根据 `enableDiscreetLogin` 配置值，有两种样式。

当您只有一个活动用户且该用户没有密码保护时，登录界面将被绕过并且不显示。

### 用户资料

您可以通过顶部菜单栏中的“用户设置”面板下的“账户”按钮访问账户自我管理菜单。

1. 显示名称 - 用于登录屏幕，可以更改。与角色无关，并且对 AI API 不可见 - 您仍然可以使用任意数量的角色。
2. 个人资料图片 - 用于登录屏幕。您可以使用自定义图片、默认角色图片（如果设置了）或最后使用的角色。
3. 密码 - 锁定图标反映账户保护状态（打开的锁 = 无密码）。可以使用“更改密码”按钮设置、更改或删除密码。
4. 设置快照 - 访问和查看您的 `settings.json` 文件的备份，并可以创建或恢复快照。
5. 下载备份 - 下载您的用户数据文件夹的归档。
6. 重置设置 - 重置为出厂默认设置，同时保留其他数据（角色、聊天）不变。

### 密码恢复

1. 可以从登录屏幕恢复密码。您需要访问服务器控制台以获取一次性恢复代码（由4位数字组成）。
2. 或者，您可以在SillyTavern服务器中使用实用程序脚本，通过提供用户句柄来重置密码。

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

### 安全检查清单

**这只是一个建议。在将您的 ST 实例投入使用之前，请咨询网络应用安全专家。**

1. 保持您的操作系统和运行时软件（如 Node.js）更新。这将确保您的系统及时获得最新的安全补丁和修复，以帮助防止潜在的漏洞。
2. 使用白名单和网络防火墙。仅允许受信任的 IP 范围访问服务器。
3. 启用基本身份验证。在您继续前端应用程序之前，它充当“主密码”。
4. 或者，配置外部身份验证。一些已知的服务有 [Authelia](https://www.authelia.com/) 和 [authentik](https://goauthentik.io/)。
5. 永远不要让管理员账户无密码。服务器在启动时会警告您是否有任何未保护的管理员账户。
6. 在本地网络之外使用隐秘登录设置。这将隐藏用户列表，防止潜在的外部人员查看。
7. 经常检查访问日志。它们会写入服务器控制台和 `access.log` 文件，并提供有关传入连接的信息，例如 IP 地址和用户代理。
8. 配置 HTTPS。对于本地主机服务器，您可以生成并使用自签名证书。否则，您可能需要部署像 [Traefik](https://traefik.io/) 或 [Caddy](https://caddyserver.com/docs/getting-started) 的代理 Web 服务器。

在以下指南中找到更多关于安全代理的信息：[反向代理 SillyTavern](https://docs.sillytavern.app/usage/st-reverse-proxy-guide/)