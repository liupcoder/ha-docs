# Nginx安装

*视频介绍：https://www.bilibili.com/video/BV12k4y1B73B/*

---

## CentOS 安装方式

!> 注意: 如果直接使用`yum install nginx`无效的话, 可以使用以下方式试试

```bash
# 安装epel-release软件包
sudo yum install epel-release -y

# 更新
yum update

# 安装nginx
yum install nginx -y
```

## Ubuntu 安装方式
```bash
sudo apt install nginx -y
```

## 修改配置

!> 注意:因为用户权限问题, 需要把配置文件内容`user nginx;`改为`user root;`

```bash
# 修改配置
nano /etc/nginx/nginx.conf
```

> 可以将配置文件写入到`/etc/nginx/conf.d/`里

> Nginx重新加载配置命令 `nginx -s reload`

> 实时监控100行Nginx错误日志命令 `tail -100f /var/log/nginx/error.log`

## HomeAssistant反向代理配置

*配置文件：/etc/nginx/conf.d/root.conf*

```nginx
server {
    listen          88;
    server_name localhost;

    location / {
            proxy_pass  http://localhost:8123;
    }

    location /api/websocket {
            proxy_pass  http://localhost:8123/api/websocket;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
    }
}
```