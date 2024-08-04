Nginx 配置文件中，`server_name` 用于定义当前 `server` 块对应的域名。当客户端发起请求时，Nginx 会根据请求的 Host 头部信息来匹配对应的 `server_name`，从而确定使用哪个配置块来处理请求。

**server_name** 支持以下几种匹配方式：精准匹配、通配符匹配、正则表达式匹配。

## 1、精准匹配

直接指定精准的域名或者 IP 地址。如，example.com，那么只有当请求的 Host 头部完全匹配这个域名时，才会使用这个 `server` 块的配置。

```
server {
    listen       80;
    server_name  www.example.com;

    #charset koi8-r;
    #access_log  logs/host.access.log  main;

    location / {
        root   html/www/prod;
        index  index.html index.htm;
    }
}
```

```
server {
    listen       80;
    server_name  test.example.com test02.example.com;

    #charset koi8-r;
    #access_log  logs/host.access.log  main;

    location / {
        root   html/www/test;
        index  index.html index.htm;
    }
}
```

## 2、通配符匹配

使用 `*` 作为通配符，匹配任意的域名。例如：`*.example.com` 可以匹配 test.example.com、api.example.com 等。

```
server {
    listen       80;
    server_name  *.example.com;

    #charset koi8-r;
    #access_log  logs/host.access.log  main;

    location / {
        root   html/www/test;
        index  index.html index.htm;
    }
}
```

## 3、正则表达式匹配

Nginx 支持使用正则表达式来定义 `server_name`，了解下正则表达式：

```
匹配模式:
    ~* 表示不区分大小写的正则匹配。
    !~ 表示区分大小写的正则不匹配。
    !~* 表示不区分大小写的正则不匹配。

基本元字符:
    . 匹配任何单个字符（不包括换行符）。
    \w 匹配字母、数字、下划线或汉字。
    \s 匹配任何空白字符，如空格、制表符。
    \d 匹配数字。
    \b 匹配单词边界。
    ^ 和 $ 分别匹配字符串的开始和结束。

量词:
    *, +, ? 分别表示前面元素可出现0次或多次、1次或多次、0次或1次。
    {n}, {n,}, {n,m} 分别表示前面元素精确出现n次、至少n次、n到m次。
    *?, +?, ??, {n,m}? 等懒惰量词，尽可能少地匹配。

特殊字符类:
    \W, \S, \D 分别匹配非字母数字下划线、非空白字符、非数字。
    \B 匹配非单词边界。

字符组:
    [^x] 匹配除了x之外的任何字符。
    [^abc] 匹配除了abc之外的任何字符。
    
匹配单个特定字符:    
    [abc] 匹配'a'、'b' 或 'c' 中的任意一个字符。
    
范围匹配:    
    [a-z] 匹配任何小写字母从'a'到'z'。
    [0-9] 匹配任何数字从'0'到'9'。
    [A-Za-z0-9] 匹配任何大小写字母或数字。

括号与捕获:
    (exp) 捕获表达式到自动编号的组中。
    (?:exp) 非捕获组，用于分组但不捕获内容。
    (?<name>exp) 或 (?'name'exp) 命名捕获组，通过名字引用捕获的内容。

预查断言:
    (?=exp) 零宽度正向先行断言，匹配exp前面的位置。
    (?<=exp) 零宽度正向回顾断言，匹配exp后面的位置。
    (?!exp) 零宽度负向先行断言，匹配后面不跟着exp的位置。
    (?<!exp) 零宽度负向回顾断言，匹配前面不跟着exp的位置。

注释:
    (?#comment) 在正则表达式中添加注释，提高可读性。
```

**正则使用注意事项：**

**1、正则匹配以 `~` 开头：** 当使用以 `~` 开头表示是一个区分大小写的正则匹配。反之不区分大小写的匹配，则应使用 `~*`；

**2、精确匹配与正则匹配区别：** 开头如果不使用 `~` 或者 `~*` 前缀，Nginx 会将 `server_name` 对应的信息判断为精准匹配；

**3、锚定符号 `^` 和 `$`：** 正则表达式中，`^` 表示字符串的开始位置，`$` 表示字符串的结束位置。

**4、`.` 的转义：** 正则表达式中，`.` 是特殊字符，代表任何单个字符（除换行符）。如果匹配字面意思的点 `.`，使用反斜线 `\` 进行转义；

**5、大括号{}：** Nginx 配置文件中，大括号 `{}` 有特殊的含义，用于定义指令块。防止解析错误，正则使用 `{}` 时，需要用双引号或者单引号包围起来。
