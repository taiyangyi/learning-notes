# 安装依赖
```
yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

> 安装 gcc

`Nginx` 是 `C` 语言开发的源码包，需要 `C` 编译器才能安装。

```
yum install -y gcc-c++
```

> 安装 `pcre` `pcre-devel`

`PCRE(Perl Compatible Regular Expressions)` 是一个 `perl` 库，包括 `perl` 兼容的正则表达式库。`Nginx` 的 `http` 模块使用 `pcre` 来解析正则表达式.

`pcre-devel` 是使用 pcre 开发的一个二次开发库。

```
yum install -y pcre pcre-devel
```

> 安装 `zlib`

`zlib` 库提供了很多种压缩和解压缩的方式，`Nginx` 使用 `zlib` 对 `http` 包的内容进行 `gzip`。

```
yum install -y zlib zlib-devel
```

> 安装 `OpenSSL`

`OpenSSL` 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 `SSL` 协议，并提供丰富的应用程序供测试或其它目的使用。

`Nginx` 不仅支持 `http` 协议，还支持 `https`，需要 `OpenSSL` 库的支持。

```
yum install -y openssl openssl-devel
```

如下图所示，表明相关依赖安装成功。
![](../images/nginx/nginx-03-1.png)

# 安装 Nginx

```
yum install -y nginx
```

# 查看是否安装成功

```
nginx -v
[root@iZbp1d8rn0652ia3bzzmioZ local]# nginx -v
nginx version: nginx/1.20.1
```

输出 `Nginx` 的版本号说明安装成功。

# 设置 `Nginx` 开机自启动

```
systemctl enable nginx
```

输出以下内容，表示已经创建了一个软连接来关联 `Nginx`，下一步执行启动 `Nginx` 了。

```
[root@iZbp1d8rn0652ia3bzzmioZ local]# systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```

# 启动 `Nginx`
```
systemctl start nginx
```

# 查看 `Nginx` 运行状态

```
systemctl status nginx
```
可以看到，`active（running）` 表明启动成功。

```
[root@iZbp1d8rn0652ia3bzzmioZ local]# systemctl start nginx
[root@iZbp1d8rn0652ia3bzzmioZ local]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Sun 2024-07-21 21:45:00 CST; 3s ago
  Process: 30644 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 30639 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 30637 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 30646 (nginx)
   CGroup: /system.slice/nginx.service
           ├─30646 nginx: master process /usr/sbin/nginx
           ├─30647 nginx: worker process
           └─30648 nginx: worker process

Jul 21 21:45:00 iZbp1d8rn0652ia3bzzmioZ systemd[1]: Starting The nginx HTTP and reverse proxy server...
Jul 21 21:45:00 iZbp1d8rn0652ia3bzzmioZ nginx[30639]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Jul 21 21:45:00 iZbp1d8rn0652ia3bzzmioZ nginx[30639]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Jul 21 21:45:00 iZbp1d8rn0652ia3bzzmioZ systemd[1]: Started The nginx HTTP and reverse proxy server.
```

# 访问验证
打开浏览器，输入 `ip` 地址，如下图，使用 `yum -y install nginx` 安装 `nginx` 初始化页面会显示 `Welcome to CentOS`。

![](../images/nginx/nginx-03-1.png)

![](../images/nginx/nginx-03-2.png)



