---
title: docker nginx 配置证书
---
- 阅读服务条款
- 是否同意certbot提供方发送邮件给你
- 在域名提供商dns解析模板添加txt解析

- 在服务器或者电脑上安装 git
- 拉取仓库` git clone https://github.com/certbot/certbot.git`
- 进入 certbot 目录执行一下命令
  ```
  ./certbot-auto certonly  -d zenglbg.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
  ```
- 输入邮箱地址

> 必须在供应商 dns 解析模块添加 txt 解析后才能确认
> 类型为 txt
> 主机为\_acme-challenge
> txt 值为上图圈起来字符串
> 通过`dig -t txt _acme-challenge.zenglbg.com @8.8.8.8`查看 txt 解析是否生效,圈文字与上图圈中文字相同即可回车![--2020-12-09---10.46.57](/content/images/2020/12/--2020-12-09---10.46.57.png)
- 回车后即可完成证书的创建
- 添加证书目录映射`/etc/letsencrypt:/etc/letsencrypt`

```
# docker-compose-yaml
services:
  nginx:
    build:
      context: ./nginx
    volumes:
      - /etc/letsencrypt:/etc/letsencrypt
```

- 配置nginx.conf
```

server {
    listen 80;
    server_name  zenglbg.com;
    return       301 https://zenglbg.com$request_uri;
}

server {
    listen 443 ssl;
    server_name  zenglbg.com;

    ssl_certificate /etc/letsencrypt/live/zenglbg.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/zenglbg.com/privkey.pem;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;

    access_log   /var/log/nginx/ghost.log;
    error_log    /var/log/nginx/ghost_error.log;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header HOST $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
	
        proxy_pass http://127.0.0.1:2368;
        proxy_redirect off;
    }
}

```