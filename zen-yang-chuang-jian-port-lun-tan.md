# 怎样创建 Port 论坛

## 使用 docker 快速运行一个 Port 服务

你需要有：一个安装了 docker 和 docker-compose 的 Linux 主机。

[how to install docker](https://docs.docker.com/engine/install/) [如何安装 docker](https://dockerdocs.cn/engine/install/)

[how to install docker-compose](https://docs.docker.com/compose/install/) [如何安装 docker-compose](https://dockerdocs.cn/compose/install/)

安装好 docker 和 docker-compose 后，在合适的位置新建一个文件夹。下面以 `~/port` 为例子。

请注意，按照英文版教程安装的 docker compose 属于 docker 插件，输入命令 `docker compose` 执行，而不是 `docker-compose`。

#### 创建 docker-compose.yml

创建一个 [`docker-compose.yml`](https://github.com/rumsystem/nft-bbs/blob/main/docker-compose.yml) 文件，保存在 `~/port/docker-compose.yml`，输入下面的内容

{% code lineNumbers="true" %}
```yaml
services:
  router:
    image: noe132/port-router:latest
    ports:
      - "${PORT:-35572}:80"
    environment:
      PORT_SERVER_HOST: 'server'
    depends_on:
      - server

  server:
    image: noe132/port-server:latest
    volumes:
      - './config.yml:/app/packages/server/config.yml:ro'
    environment:
      DB_HOST: 'postgres'
      DB_PORT: 5432
      DB_USER: 'postgres'
      DB_PASSWORD: 'e72a7e3456874163b3b715297be8a731'
      DB_DATABASE: 'port'
    depends_on:
      - postgres

  postgres:
    image: "postgres:14-alpine"
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'e72a7e3456874163b3b715297be8a731'
      POSTGRES_DB: 'port'
    volumes:
      - 'postgres-data:/var/lib/postgresql/data'

volumes:
  postgres-data:
```
{% endcode %}

第 5 行的端口号 35572 是服务最终在主机上监听的端口，根据需要可以修改为其他端口。另外也可以修改环境变量 `PORT` 修改端口。

第 19 行的 `DB_PASSWORD` 和 28 行的 `POSTGRES_PASSWORD` 请自行修改成更安全的密码。

#### 修改自定义配置：创建 config.yml

如果需要修改一些参数选项，需要创建一个 `config.yml` 文件并挂载到 `server` 容器的 `/app/packages/server/config.yml` 位置。配置文件的所有选项和默认值在 [https://github.com/rumsystem/nft-bbs/blob/main/packages/server/config.sample.yml](https://github.com/rumsystem/nft-bbs/blob/main/packages/server/config.sample.yml) 可以看到。所有的选项都是可选的。

根据上面的 `docker-compose.yml` 配置，创建一个 `config.yml` 文件，保存在 `~/port/config.yml`，输入下面的内容：

{% code lineNumbers="true" %}
```yaml
# 默认的登录选项
defaultGroup:
  mixin: true # 允许 Mixin 扫码登录
  keystore: true # 允许输入 Keystore 登录
  anonymous: true # 允许 游客模式
  metamask: true # 允许 Metamask 登录

# 管理员用户的 eth 地址
admin:
  - '0xffffffffffffffffffffffffffffffffffffffff'

# 允许用户手动输入种子网络 SeedUrl 添加新的论坛
joinBySeedUrl: false
```
{% endcode %}

#### 运行

保存完上面的文件后，使用 docker-compose 启动服务

```shell
cd ~/port
docker-compose pull
docker-compose up -d
```

#### 停止服务

```shell
docker-compose down
```

#### 查看服务运行状态

```shell
docker-compose ps
```

## 手动运行

如果不方便使用 docker，你也可以手动编译运行。你需要自己安装 nodejs / yarn / nginx / postgres 等需要的软件并配置相关的反向代理。

参考 [https://github.com/rumsystem/nft-bbs#readme](https://github.com/rumsystem/nft-bbs#readme)
