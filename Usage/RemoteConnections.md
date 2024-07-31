---
icon: rss
order: 24
---

# 远程连接

这通常是为了那些希望在手机上使用 SillyTavern 的人，同时他们的 PC 在同一 WiFi 网络内运行 ST 服务器。

不过，它也可以用于允许从任何地方进行远程连接。

默认情况下，ST 服务器仅接受本地连接。为了允许远程连接，请在 `config.yaml` 中将参数 `listen` 设置为 `true`：
```yaml
# Listen for incoming connections
listen: true
```
在激活远程连接监听后，您 **必须** 打开下面列出的至少一种限制方法，否则 ST 服务器将拒绝启动。

**注意：** SillyTavern 是一个单用户程序，因此任何登录的人都能够看到所有角色和聊天，并能够更改 UI 内的任何设置。[基本认证](#HTTPBasicAuthentication) 允许在访问 ST 之前请求授权，但单用户原则仍然适用。

## 白名单

### 1. 管理白名单IP {#managing-whitelisted-ips}

* 在您的SillyTavern基础安装文件夹中创建一个名为`whitelist.txt`的新文本文件。
* 在文本编辑器中打开该文件，添加您希望允许连接的IP列表。
  * 每个IP必须单独占一行。
  * **必须将127.0.0.1包含在列表中，否则您将无法在主机上连接**
  * 接受单个IP和通配符(*) IP范围。
  * 也接受CIDR掩码（例如10.0.0.0/24）。

示例：

```txt
192.168.0.1
192.168.0.20

//或简单地

192.168.*.*
```

（上述通配符IP范围将允许本地网络上的任何设备连接）

#### 一般用途的whitelist.txt

准确复制并粘贴以下内容：

```txt
192.168.*.*
127.0.0.1
```

这将允许与主机机器在同一网络上的任何设备，以及主机机器本身，连接到ST。

* 保存`whitelist.txt`文件。
* **重启您的SillyTavern服务器。**

*注意：`config.yaml`中也有一个`whitelist`数组，您可以以相同的方式使用，但如果存在`whitelist.txt`，则该数组将被忽略。我们不推荐使用`config.yaml`中的IP列表，因为使用`whitelist.txt`更简单*

### 2. 获取 ST 主机的 IP

在设置好白名单后，您需要 ST 托管设备的 IP 地址。

如果 ST 托管设备在同一 WiFi 网络上，您将使用 ST 主机的内部 WiFi IP：

* 对于 Windows：点击 Windows 按钮 > 在搜索栏中输入 `cmd.exe` > 在控制台中输入 `ipconfig`，按 Enter > 查找 `IPv4` 列表。

如果您（或其他人）想在不在同一网络的情况下连接到您的托管 ST，您将需要 ST 托管设备的公共 IP。

* 在使用 ST 托管设备时，访问 [此页面](https://whatismyipaddress.com/) 并查找 `IPv4`。这就是您从远程设备连接时使用的地址。

### 3. 将远程设备连接到ST主机

无论您在您的情况下得到了什么IP，您都将该IP地址和端口号输入到远程设备的网页浏览器中。

在同一wifi网络上的ST主机的典型地址如下所示：

`http://192.168.0.5:8000`

使用 http:// 而不是 https://

### 将您的 ST 开放给所有 IP

我们不建议这样做，但您可以打开 `config.yaml` 并将 `whitelistMode` 更改为 `false`。

如果存在，您必须删除（或重命名） SillyTavern 基础安装文件夹中的 `whitelist.txt`。

这通常是一种不安全的做法，因此我们要求您在这样做时设置用户名和密码。

用户名和密码在 `config.yaml` 中设置。

重启您的 ST 服务器后，任何设备都可以连接到它，只要他们知道用户名和密码，无论其 IP 地址如何。

### 仍然无法连接？

* 为 `config.yaml` 中找到的端口创建入站/出站防火墙规则。请勿将此与路由器上的端口转发混淆，否则有人可能会找到您的聊天记录，这可是个大问题。
* 在设置 > 网络和互联网 > 以太网中启用私有网络配置文件类型。这对于 Windows 11 非常重要，否则即使有上述防火墙规则，您也无法连接。

## <span id="HTTPBasicAuthentication">HTTP基本认证</span>

每当客户端通过HTTP连接时，服务器将要求输入用户名和密码。**这仅在启用了远程连接（listen: true）时有效。**

要启用HTTP基本认证，请打开SillyTavern基础目录中的`config.yaml`文件，并搜索`basicAuthMode`。将basicAuthMode设置为true，并设置用户名和密码。注意：`config.yaml`文件仅在ST至少执行过一次后才会存在。
```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

保存文件并在SillyTavern已经运行的情况下重启。如果连接到你的ST时，应该会提示输入用户名和密码。用户名和密码以明文形式传输。如果你对此感到担忧，可以通过HTTPS提供ST。

## HTTPS

### 使用 TLS/SSL 启动 SillyTavern

要加密与您的 ST 实例之间的流量，请使用 `--ssl` 标志启动服务器。

示例：
```
node server.js --ssl
```
默认情况下，ST 会在 `certs` 文件夹中查找您的证书。如果您的文件位于其他位置，可以使用 `--keyPath` 和 `--certPath` 参数。

示例：
```
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

运行 SillyTavern 的用户需要对证书文件具有读取权限。

### 如何获取证书

获取证书最简单、最快的方法是使用 [certbot](https://letsencrypt.org/getting-started/)。

## Tailscale

Tailscale 是一个 VPN 提供商，能够安全地远程连接到您的 PC。Tailscale 服务器的开源实现存在，您还可以使用 [Headscale](https://github.com/juanfont/headscale) 来托管服务器，但这超出了本教程的范围。

### 1. 创建账户

* 访问 [Tailscale 的网站](https://tailscale.com/) 并创建一个新账户。

**注意：** 对于单个用户的日常使用，Tailscale 将永久免费。如果您担心隐藏费用，只需不添加任何支付选项。

### 2. 设置客户端

* 前往 [Tailscale 的下载页面](https://tailscale.com/download)，在运行 SillyTavern 的设备和您想要从远程位置使用的设备上下载客户端/应用程序。
* 在两个设备上使用您之前创建的帐户登录。
* 前往 [Tailscale 的管理页面](https://login.tailscale.com/admin/machines)，并批准这两个设备。
* 记下这两个连接设备的名称。

### 3. 将您的设备添加到白名单

* 按照 [管理白名单 IP](#managing-whitelisted-ips) 的说明，将您要与 SillyTavern 一起使用的连接设备的机器名称添加到 SillyTavern 的白名单中。

### 4. 连接

现在，无论何时您想从任何地方使用 SillyTavern，您只需：

* 在托管 SillyTavern 的 PC 和您希望远程使用它的设备上都开启 Tailscale。
* 在想要连接的设备上打开浏览器，访问 `http://<machine name of PC running st>:8000/`

### 5. 与朋友共享SillyTavern实例（可选）

* 告诉你的朋友创建自己的Tailscale账户并在他们的设备上下载客户端。
* 前往[Tailscale的管理页面](https://login.tailscale.com/admin/machines)。
* 将鼠标悬停在托管SillyTavern的PC上的三点按钮上，然后按“共享...”或按三点按钮然后按“共享设置...”。
* 取消勾选“允许用作出口节点”（除非你希望你的朋友能够通过你的PC路由他们所有的互联网流量）。
* 通过电子邮件发送链接，或者切换到“复制共享链接”标签，按下带有相同文本的大蓝色按钮，并以其他方式将其发送给你的朋友。
* 在点击你的共享链接后，你的朋友将在他们的Tailscale网络中看到你的PC弹出。
* 将你用来访问SillyTavern的相同链接发送给你的朋友，如上一步所述。

**注意：** 这将使你的朋友完全访问你PC上本地运行的任何服务，如SillyTavern、automatic1111等。只有在你真正信任你的朋友的情况下才这样做。