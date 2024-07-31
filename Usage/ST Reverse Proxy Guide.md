---
label: 反向代理 SillyTavern
order: 23
icon: server
---

!!! danger 注意
本节内容**不**涉及 OpenAI/Claude 反向代理。仅指**HTTP/HTTPS 反向代理**。
!!!

Termux 的设置让您感到困惑吗？您是否厌倦了在每个设备上更新和安装 ST？想要对您的聊天和角色进行组织吗？那么您很幸运。本指南将_希望_涵盖如何在您的 PC 上托管 SillyTavern，您可以从任何地方连接并与您用于运行 AI 模型的同一台 PC 上的机器人聊天！

!!! warning 警告
本指南**不适合**初学者。这将非常技术性。
!!!

## 公平警告

!!! info 对于 Windows 用户
请参阅 _[安装](#installation)_ 下的 [Windows](#windows)。
!!!

!!! info 对于 Linux 用户
您必须具备以下知识

-   Linux 控制台命令
-   DNS 记录
-   公共 IP 地址
-   [Docker](https://docker.com)

!!!

**您需要为自己购买一个域名，并为您的 SillyTavern 页面配置一个 `CNAME`。我们建议在 [Cloudflare](https://cloudflare.com) 上添加或购买域名，因为本指南将涵盖如何使用 Cloudflare 本身进行此操作。**

## 安装

### Linux (裸机 SillyTavern)

对于 Linux，我们将通过 [Traefik](https://traefik.io/traefik/) 反向代理 SillyTavern。还有其他选项，如 _NGINX_ 或 _Caddy_，但在本指南中，我们将使用 Traefik，因为这是我们自己使用的。

1. 使用 `ifconfig` 或从路由器获取计算机的私有 IP。
   !!! info 提示
   建议将私有 IP 设置为静态 IP。请参考路由器手册或使用 Google 配置静态 IP。
   !!!
2. 通过 Google 搜索 `what's my ip` 获取调制解调器的公共 IP。
   !!! info 关于公共 IP
   大多数住宅/家庭网络使用 **动态 IP**，在使用几个月后会进行更新。如果您有动态 IP，请使用 DDClient，或记得在 Cloudflare 仪表板上定期检查并更改您的公共 IP。
   !!!
3. 按照 Docker 安装指南 [这里](https://docs.docker.com/engine/install/) 安装 Docker。
   !!! danger 注意
   **不要** 安装 Docker Desktop。
   !!!
4. 按照 Docker 安装后指南 [这里](https://docs.docker.com/engine/install/linux-postinstall/) 中的 **以非 root 用户管理 Docker** 步骤进行操作。
5. 前往 Linux 的根文件夹，创建一个名为 `docker` 的新文件夹。
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. 执行 `chown`，将 _<USER>_ 替换为您的 Linux 用户名，以设置 docker 文件夹的权限。
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. 在 _docker_ 文件夹内创建一个名为 `secrets` 的文件夹，在 _secrets_ 内再创建一个名为 `cloudflare` 的文件夹。
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. 在 _docker_ 文件夹内创建一个名为 `appdata` 的文件夹，在 _appdata_ 内再创建一个名为 `traefik` 的文件夹。随后进入 `appdata/traefik` 文件夹。
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. 使用 `touch` 创建一个 _acme.json_ 文件，并将其权限设置为 600。
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. 使用 `nano` 或类似的编辑器，创建一个名为 _traefik.yml_ 的文件，并粘贴以下内容。将模板电子邮件替换为您自己的，然后保存文件。
    ```yml
    api:
        dashboard: true
        debug: true
        insecure: true
    entryPoints:
        http:
            address: ":80"
            http:
                redirections:
                    entryPoint:
                        to: https
                        scheme: https
        https:
            address: ":443"
    serversTransport:
        insecureSkipVerify: true
    providers:
        docker:
            endpoint: "unix:///var/run/docker.sock"
            exposedByDefault: false
        file:
            filename: /config.yml
            watch: true
    certificatesResolvers:
        cloudflare:
            acme:
                email: YOUR_CLOUDFLARE_EMAL@DOMAIN.com
                storage: acme.json
                dnsChallenge:
                    provider: cloudflare
                    #disablePropagationCheck: true  # 如果您在通过 cloudflare 拉取证书时遇到问题，请取消注释此行，将此标志设置为 true 可禁用等待 TXT 记录传播到所有权威名称服务器的需要。
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. 返回 `docker` 文件夹。
    ```sh
    cd /docker
    ```
12. 使用 `nano` 或类似的编辑器，创建一个名为 _docker-compose.yaml_ 的文件，并粘贴以下内容。随后保存文件。

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro

    networks:
        internal:
            driver: bridge
    ```

13. 登录到 Cloudflare，点击您的域名，然后点击 **获取您的 API 令牌**。
14. 点击 _创建令牌_ 然后 _创建自定义令牌_，确保您为令牌授予以下权限。
    !!! info 令牌权限
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _继续到摘要_，然后点击 _创建令牌_。

15. 复制给您的令牌密钥并将其安全存储。
16. `cd` 到 `secrets/cloudflare`，使用 `nano` 或类似的编辑器，创建一个名为 **CF_DNS_API_KEY** 的文件并粘贴您的密钥。
17. 返回到您的域名页面，转到 **DNS**。使用 **添加记录** 创建一个新记录，并创建两个 _A_ 类型的记录，如下所示。将 `PUBLIC_IP` 替换为您自己的公共 IP，然后点击 _保存_。

    | 类型 | 名称（必填） | 目标（必填） | 代理状态 | TTL  |
    | ---- | ------------- | -------------- | ---------- | ---- |
    | A    | DOMAIN.com    | PUBLIC_IP      | 代理       | 自动 |
    | A    | www           | PUBLIC_IP      | 代理       | 自动 |

18. 创建另一个 **`CNAME`** 类型的记录，然后点击 _保存_。以下是在 Cloudflare 仪表板上应该如何显示的示例。

    | 类型  | 名称（必填） | 目标（必填） | 代理状态 | TTL |
    | ----- | ------------- | -------------- | ---------- | --- |
    | CNAME | silly         | DOMAIN.com     | 代理       | N/A |

19. `cd` 到 _appdata/traefik_，使用 `nano` 或类似的编辑器，创建一个名为 _config.yml_ 的文件并粘贴以下内容。将 `PRIVATE_IP` 替换为您获取的私有 IP，将 `silly.DOMAIN.com` 替换为您的子域名和域名页面，然后保存文件。

    ```yml
    http:
        routers:
            sillytavern:
                entryPoints:
                    - "https"
                rule: "Host(`silly.DOMAIN.com`)"
                middlewares:
                    - https-redirectscheme
                tls: {}
                service: sillytavern

        services:
            sillytavern:
                loadBalancer:
                    servers:
                        - url: "http://PRIVATE_IP:8000"
                    passHostHeader: true

        middlewares:
            https-redirectscheme:
                redirectScheme:
                    scheme: https
                    permanent: true
    ```

20. 使用以下命令运行 Docker Compose：
    ```sh
    cd /docker
    docker compose up -d
    ```
21. 转到您的 SillyTavern 文件夹，编辑 `config.yaml` 以启用监听模式和基本身份验证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning 提示
    确保将默认用户名和密码更改为您可以记住的强密码。
    !!!

22. 等待几分钟，然后打开您为 ST 创建的域名页面。在页面的末尾，您应该能够通过一个 URL 和一个帐户从任何地方访问 SillyTavern。
    !!! info 提示
    如果几分钟后没有任何反应，请检查 Traefik 的容器日志以查找可能的错误。
    !!!
23. 享受吧！ :D

### Linux (Docker SillyTavern)

!!! warning 注意
请注意，我们在裸金属上通过Docker运行SillyTavern。这是我们在Docker上与其他Docker容器一起使用ST时的大致步骤。
!!!

1. 按照**Linux (裸金属SillyTavern)**的步骤1-11进行操作。
2. 登录到Cloudflare，点击您的域名，然后选择**获取您的API令牌**。
3. 点击_创建令牌_，然后_创建自定义令牌_，确保为您的令牌授予以下权限。
   !!! info 令牌权限
   **区域 -> DNS -> 编辑**

    **区域 -> 区域 -> 读取**
    !!!

    点击_继续到摘要_，然后_创建令牌_。

4. 复制给您的令牌密钥并将其存储在安全的地方。
5. `cd`进入`secrets/cloudflare`，使用`nano`或类似编辑器，创建一个名为**CF_DNS_API_KEY**的文件，并将您的密钥粘贴进去。
6. 返回到您的域名页面，转到**DNS**。使用**添加记录**创建一个新记录，并创建两个_A_类型的记录，如下所示。将`PUBLIC_IP`替换为您的公共IP，将示例域名替换为您的域名，然后点击_保存_。

    | 类型 | 名称（必填） | 目标（必填） | 代理状态 | TTL  |
    | ---- | --------------- | ----------------- | ------------ | ---- |
    | A    | DOMAIN.com      | PUBLIC_IP         | 代理        | 自动 |
    | A    | www             | PUBLIC_IP         | 代理        | 自动 |

7. 创建另一个**`CNAME`**类型的记录，然后点击_保存_。以下是它在Cloudflare仪表板上应显示的示例。

    | 类型  | 名称（必填） | 目标（必填） | 代理状态 | TTL |
    | ----- | --------------- | ----------------- | ------------ | --- |
    | CNAME | silly           | DOMAIN.com        | 代理        | N/A |

8. 将SillyTavern克隆到`docker`文件夹中。
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. 使用`nano`或类似编辑器，创建一个名为_docker-compose.yaml_的文件，并粘贴以下内容。将`silly.DOMAIN.com`替换为您上面添加的子域，然后保存文件。

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro
        sillytavern:
            build: ./SillyTavern
            container_name: sillytavern
            hostname: sillytavern
            image: ghcr.io/sillytavern/sillytavern:latest
            volumes:
                - "./appdata/sillytavern/config:/home/node/app/config"
                - "./appdata/sillytavern/data:/home/node/app/data"
            restart: unless-stopped
            labels:
                - "traefik.enable=true"
                - "traefik.http.routers.sillytavern.entrypoints=http"
                - "traefik.http.routers.sillytavern.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.middlewares.sillytavern-https-redirect.redirectscheme.scheme=https"
                - "traefik.http.routers.sillytavern.middlewares=sillytavern-https-redirect"
                - "traefik.http.routers.sillytavern-secure.entrypoints=https"
                - "traefik.http.routers.sillytavern-secure.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.routers.sillytavern-secure.tls=true"
                - "traefik.http.routers.sillytavern-secure.service=sillytavern"
                - "traefik.http.services.sillytavern.loadbalancer.server.port=8000"

    networks:
        internal:
            driver: bridge
    ```

10. 使用以下命令运行Docker Compose：
    ```sh
    docker compose up -d
    ```
11. 停止SillyTavern Docker容器。
    ```sh
    docker compose stop sillytavern
    ```
12. 转到您的SillyTavern文件夹（`appdata/sillytavern/config`）并编辑`config.yaml`以启用监听模式和基本身份验证，同时禁用`whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning 提示
    确保将默认用户名和密码更改为您能记住的强密码。
    !!!

13. 再次启动SillyTavern Docker容器。
    ```sh
    docker compose up -d sillytavern
    ```
14. 等待几分钟，然后打开您为ST创建的域名页面。在页面的末尾，您应该能够通过一个URL和一个账户从任何地方访问SillyTavern。
    !!! info 提示
    如果几分钟后没有任何反应，请检查Traefik的容器日志以查找可能的错误。
    !!!
15. 享受吧！ :D

### Windows

在Windows中，反向代理略有不同。目前，您在Windows上可用的唯一选项（根据我们所知）是通过在路由器上运行VPN或使用Cloudflare/NGROK隧道。有关更多信息，请参阅[**其他选项**](#other-options)。

!!! info 今日事实
_您知道Windows比Linux消耗更多的VRAM吗？如果您需要更多的上下文或更高参数的模型，我们建议双启动Windows和Linux，或者选择纯Linux以节省一些VRAM。_
!!!

## 可选内容

虽然我们不一定会详细介绍这些内容，但如果您想更好地保护您的 SillyTavern 实例，以下是一些建议尝试使用的东西。（内容会在适当时添加）

1. 使用 [**Authelia**](https://www.authelia.com/) 或 [**Authentik**](https://goauthentik.io/)。

    Authelia/Authentik 是一个开源的单点登录 (SSO) 应用程序，允许您创建用户并通过在您想要保护的网站上呈现的登录门户来保护许多不同的页面。我们中的一位主要在其自己的域名使用此应用，而不是 ST 的基本认证，虽然设置起来比较复杂，但这是学习 SSO 和在互联网上更好地保护您的 ST 实例的好方法。

    如果您计划在 ST 的基本认证上使用 Authelia，您必须在 _config.yaml_ 中禁用 `basicAuthMode` 并启用 `securityOverride`。

2. 使用 [**DDClient**](https://ddclient.net/)。

    DDClient 允许您将公共 IP 同步到 Cloudflare，以防您的 ISP 更改它，从而使您能够继续像以前一样访问您的 ST 实例。

## 其他选项

如果反向代理对你来说有点复杂，还有其他方法可以连接到你的 ST 实例。

1. 使用 **自制 VPN**。

    一些路由器具备在路由器管理页面中托管 VPN 服务器（主要是 OpenVPN 或 WireGuard）的能力。请参考路由器的手册设置 VPN 并将你的设备添加到 VPN 中。连接后，只需访问为 SillyTavern 设置的私有 IP，就可以正常连接。这对用户和 Windows 使用更方便。

2. 使用 [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)。

    Cloudflare Zero Trust 是 Cloudflare 中的一个免费组织功能，允许你添加 50 个用户。它会通过 Cloudflare 代理你的流量，通过使用 `cloudflared` 将你的 ST PC 添加为隧道，你可以像在家一样连接到你的 ST 实例。

    请注意，在创建隧道后，你需要向路由器的私有 IP 地址添加路由，并计算 IP CIDR 值，以便在使用 Cloudflare Zero Trust 时能够完全本地访问。

3. 使用独立的 **Cloudflare/NGROK 隧道**。

    类似于 AI 后端的连接方式，你也可以通过 Cloudflare 隧道连接你的 ST 实例，并打开 Cloudflare 隧道页面。然而，每次想要在移动中使用 ST 时，你都需要复制并粘贴 Cloudflare/NGROK 生成的每个新链接。