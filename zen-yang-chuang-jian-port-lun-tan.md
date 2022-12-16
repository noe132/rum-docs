# 怎样创建 Port 论坛

## 使用 docker 快速运行一个 Port 服务

你需要有：一个安装了 docker 和 docker-compose 的 Linux 主机。

[how to install docker](https://docs.docker.com/engine/install/) [如何安装 docker](https://dockerdocs.cn/engine/install/)

[how to install docker-compose](https://docs.docker.com/compose/install/) [如何安装 docker-compose](https://dockerdocs.cn/compose/install/)

安装好 docker 和 docker-compose 后，在合适的位置新建一个文件夹。下面以 `~/port` 为例子。

请注意，按照英文版教程安装的 docker compose 属于 docker 插件，输入命令 `docker compose` 执行，而不是 `docker-compose`。

#### 创建 docker-compose.yml

创建一个 `docker-compose.yml` 文件，保存在 `~/port/docker-compose.yml`，输入下面的内容

{% code lineNumbers="true" %}
```yaml
version: "3.7"

services:
  router:
    image: noe132/port-router:latest
    ports:
      - "${PORT:-35572}:80"
    volumes:
      - './router.nginx.conf:/etc/nginx/nginx.conf'
    depends_on:
      - server

  server:
    image: noe132/port-server:latest
    volumes:
      - './config.yml:/app/packages/server/config.yml'
    environment:
      PORT: 80
    depends_on:
      - postgres

  postgres:
    image: "postgres:14-alpine"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: e72a7e3456874163b3b715297be8a731
      POSTGRES_DB: port
    volumes:
      - 'postgres-data:/var/lib/postgresql/data'

volumes:
  postgres-data:

```
{% endcode %}

第 7 行的端口号 35572 是服务最终在主机上监听的端口，根据需要可以修改为其他端口。

第 8 行的 `POSTGRES_PASSWORD` 请自行修改成更安全的密码。

#### 创建 router.nginx.conf

创建一个 `router.nginx.conf`，保存在 `~/port/router.nginx.conf`，输入下面的内容

```nginx
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    keepalive_timeout  65;

    upstream portserver {
        server server:80;
    }

    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        location / {
            root   /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }

        location /api {
            proxy_pass http://portserver;
        }

        location /socket.io/ {
            proxy_pass http://portserver;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "Upgrade";
            proxy_set_header Host $host;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
    }

    include /etc/nginx/conf.d/*.conf;
}
```

#### 创建 config.yml

创建一个 `config.yml` 文件，保存在 `~/port/config.yml`，输入下面的内容

{% code lineNumbers="true" %}
```yaml
# database connection configuration
db:
  host: postgres
  port: 5432
  database: port
  user: postgres
  password: 'e72a7e3456874163b3b715297be8a731'
  dialect: postgres

# default login configuration
defaultGroup:
  mixin: true
  keystore: true
  anonymous: true
  metamask: true

# admin user eth addresses
admin:
  - '0xffffffffffffffffffffffffffffffffffffffff'

# allow user add group by seedUrl
joinBySeedUrl: false

```
{% endcode %}

请注意第 7 行的 `password` 需要跟 `docker-compose.yml` 中的 `POSTGRES_PASSWORD` 保持一致。

####

#### 运行

保存完上面的3个文件后，使用 docker-compose 启动服务

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

和上面步骤的 nginx.conf
