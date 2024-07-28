
# nginx目录结构

```
[root@iZbp1d8rn0652ia3bzzmioZ nginx]# pwd
/usr/local/nginx
[root@iZbp1d8rn0652ia3bzzmioZ nginx]# tree -L 2
.
├── client_body_temp
├── conf # 配置文件目录
│?? ├── fastcgi.conf
│?? ├── fastcgi.conf.default
│?? ├── fastcgi_params
│?? ├── fastcgi_params.default
│?? ├── koi-utf
│?? ├── koi-win
│?? ├── mime.types
│?? ├── mime.types.default
│?? ├── nginx.conf # 主要配置文件，该文件可以引用其它配置文件
│?? ├── nginx.conf.default
│?? ├── scgi_params
│?? ├── scgi_params.default
│?? ├── uwsgi_params
│?? ├── uwsgi_params.default
│?? └── win-utf
├── fastcgi_temp
├── html # 静态资源目录
│?? ├── 50x.html # Nginx访问出现错误时的提示页面
│?? └── index.html # Nginx默认访问的文件
├── logs # 日志目录
│?? ├── access.log # Nginx用户访问日志文件，所以访问都将记录在该文件中
│?? ├── error.log # Nginx访问报错时记录日志文件
│?? └── nginx.pid # 记录Nginx的主进程的Pid的文件
├── proxy_temp
├── sbin
│?? └── nginx # Nginx程序启动文件
├── scgi_temp
└── uwsgi_temp

9 directories, 21 files

```

## conf 目录

`conf` 目录是 Nginx 服务器主要配置文件目录，这个目录的位置取决于 Nginx 的安装方式和操作系统。默认情况下，源代码编译并安装的 Nginx，conf 目录位于 Nginx 安装目录下的 conf 子目录中。

在 `conf` 目录中，`nginx.conf` 是 Nginx 的主配置文件。此外，该目录可能还包含其他配置文件，如 `mime.types`（定义MIME类型）、`fastcgi_params`（FastCGI参数配置）、`koi-utf`、`koi-win`、`win-utf`（字符编码转换映射文件）等。

另外，`conf` 目录下可能还有一个`sites-available` (虚拟主机配置文件目录，用于存放每个虚拟主机的配置文件)和 `sites-enabled` (启用的虚拟主机配置文件目录的符号链接，通过链接到 sites-available 下的配置文件来启用虚拟主机)，用于存放和启用特定的网站或应用配置。这种结构允许我们轻松地管理和`启用`/`禁用`不同的网站配置。

## html 目录

`html` 目录是 Nginx 服务器的默认网站目录，它包含了一些示例网站和静态文件，这些文件可以被 Nginx 服务器作为静态文件处理。默认情况下，Nginx 会在 `html` 目录中查找名为 `index.html` 的文件，并将其作为默认网站的首页显示。

这个目录通常包含了网站的 `HTML`、`CSS`、`JavaScript` 文件、图片以及其他静态资源。当 Nginx 服务器收到客户端的请求时，它会从这个目录中查找并返回相应的文件。

> **html 目录使用归纳：**

- **位置**：Nginx 的默认站点目录通常位于 Nginx 安装目录下的 html 子目录中。在 Linux 系统中，这个位置可能会因安装方式和操作系统的不同而有所变化；
  
- **内容**：html 目录主要包含网站的静态文件，如 HTML 页面、CSS 样式表、JavaScript 脚本、图片等。这些文件是构成网站前端界面的基础元素；
  
- **配置**：在 Nginx 的配置文件中，可以指定网站根目录的位置。通过修改这个配置，你可以将 Nginx 的默认站点目录更改为其他位置；
  
- **访问**：当用户通过浏览器访问 Nginx 服务器时，Nginx 会根据请求的路径从 html 目录（或其他配置的站点目录）中查找并返回相应的文件。如果文件不存在，Nginx 通常会返回一个 404 错误页面；
  
- **权限**：为了确保 Nginx 服务器能够正常访问和操作 html 目录中的文件，需要确保 Nginx 进程对该目录及其文件具有适当的读写权限；


## logs 目录

`logs` 目录是 nginx 日志的存放目录，日志包含了 Nginx 服务器处理的请求信息、运行状态信息以及一些可能发生错误的重要信息。其中 `access.log` 是访问日志，`error.log` 是错误日志，`nginx.pid` 是主进程的进程 ID。

如果配置了多个网站或者应用，并且为每个网站或者应用指定了不同的日志文件，那么这些日志文件也将存放在 `logs` 目录中。为了管理和维护 Nginx 服务器，定期检查和分析日志文件是非常有必要且重要的执行流程。它可以帮助我们了解服务器的性能、安全性以及任何潜在的问题。同时，通过配置日志文件的轮转（rotation）和压缩，可以确保日志文件不会无限增长，从而占用大量的磁盘空间。避免出现用户访问故障，请定时定期清理和归档日志文件。

> **access.log 访问日志**

访问日志，记录客户端访问服务器的每一条请求的信息，如请求的 IP 地址、时间戳、请求的方法（GET、POST 等）、请求的 URL、HTTP 响应状态码等。

```
112.10.213.217 - - [28/Jul/2024:17:31:06 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36"
```

> **error.log 错误日志**

错误日志，记录 Nginx 服务器在处理请求时遇到的任何错误或警告信息。

```
2024/07/28 16:52:16 [error] 1550#1550: *409 open() "/usr/local/nginx/html/favicon.ico" failed (2: No such file or directory), client: xxx.133.130.33, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "xx.xx.xx.xx:80"
```

> **nginx.pid 主进程的进程 ID**

记录主进程的进程 ID

```
[root@iZbp1d8rn0652ia3bzzmioZ logs]# cat nginx.pid 
1549

[root@iZbp1d8rn0652ia3bzzmioZ logs]# ps -ef|grep nginx
root      1549     1  0 02:08 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
nobody    1550  1549  0 02:08 ?        00:00:00 nginx: worker process
root     14838 13996  0 17:36 pts/0    00:00:00 grep --color=auto nginx
```

## sbin 目录

`sbin` 目录是 Nginx 服务器的可执行文件目录，其中包含了 Nginx 服务器的主要可执行文件，如 `nginx`、`nginx-debug`、`nginx-check` 等。这些可执行文件是 Nginx 服务器的核心组件，负责处理客户端的请求并生成相应的响应。

在 Linux 系统中，源代码编译并安装了 Nginx，`sbin` 目录通常位于 Nginx 的安装目录下的 sbin 子目录中，如 `/usr/local/nginx/sbin/`。通过包管理器（如 apt、yum 或 dnf）安装的 Nginx，`sbin` 目录可能位于系统的标准命令目录中，但通常会有指向该目录中nginx命令的符号链接（symlink）位于 `/usr/sbin/`或 `/usr/bin/`；


> **启动 Nginx：**

通过执行sbin目录下的nginx可执行文件来启动Nginx服务器。命令通常如下（以绝对路径为例）：`./usr/local/nginx/sbin/nginx`；

> **停止 Nginx：**

使用带有-s stop参数的nginx命令来优雅地停止Nginx服务器。例如：`./usr/local/nginx/sbin/nginx -s stop`；

> **重新加载配置：**

如果修改了 Nginx 的配置文件，可以使用带有 `-s reload` 参数的  `Nginx` 命令来重新加载配置，而无需重启Nginx。例如：`./usr/local/nginx/sbin/nginx -s reload`；

> **查看版本：**

通过执行带有 `-v` 或者 `-V` 参数的nginx命令来查看当前安装的 Nginx 版本。如：`./usr/local/nginx/sbin/nginx -v`；

> **检查配置文件：**

使用带有 -t 参数的 Nginx 命令来检查 Nginx 的配置文件是否有语法错误。例如：`./usr/local/nginx/sbin/nginx -t`；

> **其他命令和选项：**

`sbin` 目录下的 Nginx 可执行文件还支持其他选项和参数，用于更精细地控制 Nginx 服务器的行为，请参考：Nginx 官方文档；

> **管理Nginx进程：**

使用Linux系统的进程管理工具（如ps、kill等）来查看和管理 Nginx 的进程。例如，使用 `ps -ef | grep nginx` 命令可以查看正在运行的 Nginx 进程；

> **权限和安全性：**

由于 `sbin` 目录中的 `Nginx` 命令用于管理 `Nginx` 服务器，因此应该确保只有具有适当权限的用户才能访问和执行这些命令。通常，这些命令只能由 `root` 用户或具有 `sudo` 权限的用户执行；

**具体说明如下：**

> **_temp 字段的文件**

目录中带有 `_temp` 的字段是 Nginx 运行之后产生的临时文件，这些文件是由 Nginx 服务器创建和管理的，用于存储一些临时数据或中间结果。这些文件通常会在 Nginx 服务器重启或重新加载配置时被删除。

> **client_body_temp**

存储客户端请求体的临时文件。当客户端发送 POST 请求且请求体较大时，Nginx 会先将请求体写入到这个目录下的临时文件中，然后再由 Nginx 处理或转发给后端服务器。

> **fastcgi_temp**

用于 FastCGI 请求的临时文件存储。当 Nginx 作为前端服务器，与后端 FastCGI 应用服务器（如 PHP-FPM ）交互时，可能会需要存储一些临时数据，比如上传的文件或者大请求体。

> **proxy_temp HTTP**

服务于 HTTP 代理和反向代理请求的临时文件。当 Nginx 作为代理服务器转发请求到后端，并且需要暂存请求或响应的内容时，会用到这个目录。

> **scgi_temp 和 uwsgi_temp**

分别用于 SCGI（Simple Common Gateway Interface）和 uWSGI（Universal Web Server Gateway Interface）协议相关的临时文件存储。这两种协议类似于 FastCGI，用于与不同的后端应用程序服务器通信。

临时文件夹的存在，使得 Nginx 能够高效处理各种类型的请求，尤其是在处理大数据量传输时，避免了直接在内存中存储大量数据，从而降低了系统内存的压力。需要注意的是，这些目录通常由 Nginx 服务运行的用户拥有权限，以确保安全性和隔离性。**在生产环境中，定期清理这些临时文件是非常重要的维护操作，以避免磁盘空间被无用的临时文件耗尽。**