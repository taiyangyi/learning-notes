### 介绍
`scp` 命令是用于通过 `SSH协议` 进行远程服务与本地数据传输的命令，使用 `SCP` 在跨两端传输数据是比较安全的，推荐大家使用。

### 命令说明
#### 1、传输本地文件到服务器

```
# scp 上传文件 用户名@服务器ip:目标路径
scp /local-path/文件 server_username@server_ip:/server-path/
# 例如
scp target/xxxx.war root@192.168.1.50:/home/admin/tomcat/
```
#### 2、传输本地文件夹到服务器

```
# scp 文件夹目录 用户名@服务器ip:目标路径
scp -r /local-path/folder server_username@server_ip:/server-path/
# 例如
scp -r target/ root@192.168.1.50:/home/admin/tomcat/
```

#### 3、下载服务器文件到本地

```
# scp 用户名@服务器ip:文件路径 目标路径
scp server_username@server_ip:/server-path/xxx.x /local-path/
# 例如
scp root@192.168.1.50:/home/admin/tomcat/xxxx.war /target/
```

#### 4、下载服务器文件夹到本地

```
# scp 用户名@服务器ip:文件路径 目标路径
scp -r server_username@server_ip:/server-path/ /local-path/
# 例如
scp -r root@192.168.1.50:/home/admin/tomcat/ /target/
```
#### 5、scp命令指定了端口

```
# scp -P 端口号 文件路径 用户名@服务器ip:文件保存路径
scp -P port /file/path root@server_ip:/path/
# 例如
scp -P port root@192.168.1.50:/home/admin/tomcat/xxxx.war /target/
```
### 参数说明

- -1： 强制scp命令使用协议ssh1
- -2： 强制scp命令使用协议ssh2
- -4： 强制scp命令只使用IPv4寻址
- -6： 强制scp命令只使用IPv6寻址
- -B： 使用批处理模式（传输过程中不询问传输口令或短语）
- -C： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
- -p：保留原文件的修改时间，访问时间和访问权限
- -q： 不显示传输进度条
- -r： 递归复制整个目录
- -v：详细方式显示输出，scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题
- -c cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh
- -F ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh
- -i identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh
- -l limit： 限定用户所能使用的带宽，以Kbit/s为单位
- -o ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式
- -P port：注意是大写的P, port是指定数据传输用到的端口号
- -S program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项









