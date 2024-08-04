## 1、`nginx: [error] open() "/usr/local/nginx/logs/nginx.pid" failed (2: No such file or directory)` Nginx 中 nginx.pid 文件丢失

### 原因分析

Nginx 被停止 `nginx -s stop` 或者直接杀掉了进程 `kill -9 nginx的进程号` 后，调用命令 `nginx -s reload` 或者 `nginx -s reopen` 会报错：无法找到 `/usr/local/nginx/logs/nginx.pid` 文件。

### 解决办法

1、在 `/usr/local/nginx/logs` 下新建 `nginx.pid` 文件

```
[root@iZbp1d8rn0652ia3bzzmioZ logs]# ls -l
总用量 788
-rw-r--r-- 1 root root 438398 8月   4 10:51 access.log
-rw-r--r-- 1 root root 352499 8月   4 10:51 error.log
-rw-r--r-- 1 root root      0 8月   4 11:00 nginx.pid

```

2、查看当前系统 nginx 的 PID，写入新建的 nginx.pid 中

```
[root@iZbp1d8rn0652ia3bzzmioZ logs]# ps -ef|grep nginx
root      6815     1  0 7月29 ?       00:00:00 nginx: master process ./nginx
nobody    6816  6815  0 7月29 ?       00:00:00 nginx: worker process
root     11831  1180  0 11:01 pts/0    00:00:00 grep --color=auto nginx
```
如上，PID 为 6815，写入文件
```
[root@iZbp1d8rn0652ia3bzzmioZ logs]# echo "6815" > nginx.pid 
[root@iZbp1d8rn0652ia3bzzmioZ logs]# cat nginx.pid 
6815
```

3、在 `/usr/local/nginx/sbin` 关闭再启动

```
[root@iZbp1d8rn0652ia3bzzmioZ sbin]# ./nginx -s stop
[root@iZbp1d8rn0652ia3bzzmioZ sbin]# ps -ef|grep nginx
root     17327 13419  0 11:06 pts/0    00:00:00 grep --color=auto nginx
[root@iZbp1d8rn0652ia3bzzmioZ sbin]# systemctl start nginx
[root@iZbp1d8rn0652ia3bzzmioZ sbin]# systemctl status nginx
● nginx.service - nginx - web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since 日 2024-08-04 11:06:52 CST; 9s ago

```