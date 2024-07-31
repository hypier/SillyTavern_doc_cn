---

# icon: container
label: Docker
---

# Docker 安装

!!! info
本指南假设您在非根（非管理员）文件夹中安装了 SillyTavern。如果您在根文件夹中安装了 SillyTavern，则可能需要以管理员权限运行某些命令 [`sudo`, `doas`, Command Prompt (Administrator)]。
!!!

## 安装

### Linux

1. 按照 Docker 安装指南 [这里](https://docs.docker.com/engine/install/) 安装 Docker。
   !!! danger
   **不要** 安装 Docker Desktop。
   !!!
2. 按照 Docker [后安装指南](https://docs.docker.com/engine/install/linux-postinstall/) 中的 **以非根用户管理 Docker** 的步骤进行操作。
3. 使用你的包管理器安装 [Git](https://git-scm.com/download/linux)。

    - Debian（Ubuntu/Pop! OS 等）

        ```sh
        sudo apt install git
        ```

    - Arch Linux（Manjaro/EndeavourOS 等）

        ```sh
        sudo pacman -S git
        ```

    - Fedora、Red Hat Enterprise Linux（RHEL）等
        ```sh
        sudo dnf install git
        ```

4. 克隆 SillyTavern 仓库。

    - 发布版（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - 预发布版（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. 在 Docker 文件夹中运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

6. 执行以下 Docker 命令以获取 SillyTavern Docker 容器的 IP。

    ```sh
    docker network inspect docker_default
    ```

    你应该会收到类似以下的输出。

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    记下你在 _Gateway_ 中看到的 IP，因为这将非常重要。

7. 使用 `sudo` 打开 `nano` 并运行以下命令。

    ```sh
    sudo nano config/config.yaml
    ```

    在 `nano` 中，找到 `whitelist`。你应该会看到类似以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下方添加新的一行，并输入你从 Docker 复制的 IP。之后应该看起来类似以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    通过按 _Ctrl+S_ 保存文件，然后按 _Ctrl+X_ 退出 `nano`。

!!! info
注意，如果你将 Docker 网络配置为桥接模式，你也可以像往常一样将外部 IP 地址添加到白名单中。
!!!

9. 重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```

10. 打开一个新的浏览器，访问 [http://localhost:8000](http://localhost:8000)。你应该会在几秒钟内看到 SillyTavern 加载。

11. 享受吧！ :D

### Windows

!!! warning 关于 Windows 上的 Docker
在 Windows 上使用 Docker **_真的_** 很复杂。你不仅需要在 _打开或关闭 Windows 功能_ 中激活 _Windows 子系统 for Linux_，还需要根据 PC 制造商（或主板制造商）配置你的系统以支持虚拟化（Intel VT-d/AMD SVM）。有时，这个选项在某些系统上是不可用的。

强烈建议你按照我们的 [Windows](/Installation/Windows.md) 指南安装 SillyTavern。本节提供了在 Windows 上完成此操作的 _粗略_ 想法。
!!!

1.  按照 Docker 安装指南 [这里](https://docs.docker.com/desktop/install/windows-install/) 安装 Docker Desktop。
2.  安装 [Git for Windows](https://git-scm.com/download/win)。
3.  克隆 SillyTavern 仓库。

    -   发行版（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   暂存版（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹中运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  执行以下 Docker 命令以获取你的 SillyTavern Docker 容器的 IP。

    ```sh
    docker network inspect docker_default
    ```

    你应该会收到类似以下的输出。

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    记下你在 _Gateway_ 中看到的 IP，因为这将很重要。

6.  以管理员权限运行 _记事本_ 或你选择的代码编辑器，前往 `config` 并打开 _config.yaml_。

    在你选择的编辑器中，你应该会看到类似以下的内容。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下面添加新的一行，并输入你从 Docker 中复制的 IP。之后应该类似于以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    按 _Ctrl+S_ 保存文件，然后退出编辑器。

!!! info
请注意，如果你将 Docker 网络配置为桥接模式，你也可以像往常一样将外部 IP 地址添加到白名单中。
!!!

7.  重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```

8.  打开一个新的浏览器，访问 [http://localhost:8000](http://localhost:8000)。你应该会看到 SillyTavern 在几秒钟内加载。

9.  享受吧！ :D

### macOS

!!!
尽管 macOS 与 Linux 相似，但它没有 Docker Engine。您需要像在 Windows 上一样安装 Docker Desktop。
您还需要安装 [Homebrew](https://brew.sh/)，以便在 Mac 上安装 Git。本节提供了在 macOS 上如何进行的 _粗略_ 指南。
!!!

1.  按照 Docker 安装指南 [这里](https://docs.docker.com/desktop/install/mac-install/) 安装 Docker Desktop。
2.  使用 Homebrew 安装 `git`。

    ```sh
    brew install git
    ```

3.  克隆 SillyTavern 仓库。

    -   发布（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   预发布（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹内运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  执行以下 Docker 命令以获取您的 SillyTavern Docker 容器的 IP。

    ```sh
    docker network inspect docker_default
    ```

    您应该会收到类似以下的输出。

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    记下您在 _Gateway_ 中看到的 IP，因为这很重要。

6.  使用 `sudo` 打开 `nano` 并运行以下命令。

    ```sh
    sudo nano config/config.yaml
    ```

    !!!
    如果您无法运行 `nano`，请通过 Homebrew 安装它或使用 TextEdit。
    !!!

    在 `nano` 中，找到 `whitelist`。您应该会看到类似以下的内容。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下方添加新的一行，并输入您从 Docker 复制的 IP。之后应该类似于以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    按 _Ctrl+S_ 保存文件，然后按 _Ctrl+X_ 退出 `nano`。

!!! info
请注意，如果您将 Docker 网络配置为桥接，您也可以像往常一样将外部 IP 地址添加到白名单中。
!!!

7.  重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```

8.  打开新的浏览器并访问 [http://localhost:8000](http://localhost:8000)。您应该会在几秒钟内看到 SillyTavern 加载。

9.  享受吧！ :D

## 配置 SillyTavern

SillyTavern 的配置文件 (config.yaml) 将位于 `config` 文件夹内。配置配置文件与不使用 Docker 的配置方式没有区别，但您需要以管理员权限运行 `nano` 或代码编辑器以保存更改。

!!! warning
不要忘记重启 SillyTavern 的 Docker 容器以应用您的更改！确保您在 `docker` 文件夹内执行此命令。

```sh
docker compose restart sillytavern
```

## 定位用户数据

SillyTavern 的数据文件夹将在 `data` 文件夹内。备份您的文件应该很简单，但恢复或添加内容可能需要您具有管理员权限。

## 运行服务器插件

在 Docker 中运行插件，如 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) 或 [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)，与在没有 Docker 的情况下在您的系统上运行没有区别，但我们需要对 Docker Compose 脚本进行一些小修改。

!!! 注意
如果您已经在 `docker` 文件夹中看到 _plugins_ 文件夹，可以跳过步骤 1-2。
!!!

1. 使用 `nano` 或代码编辑器打开 _docker-compose.yml_ 并在 `volumes` 下面添加以下行。

```sh
    volumes:
        - "./config:/home/node/app/config"
        - "./data:/home/node/app/data"
        - "./plugins:/home/node/app/plugins"
```

2. 在 `docker` 文件夹中创建一个名为 _plugins_ 的新文件夹。
3. 按照您的插件说明安装插件。
4. 使用具有管理员权限的 `nano` 或代码编辑器打开 _config.yaml_（在 `config` 文件夹中）并启用 `enableServerPlugins`

```sh
enableServerPlugins: true
```

5. 重启 Docker 容器。

```sh
docker compose restart sillytavern
```

6. 获利。