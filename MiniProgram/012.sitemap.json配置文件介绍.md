## 一、sitemap.json 文件

配置小程序及其页面是否允许被微信索引，提高小程序在微信内部被用户搜索到的概率。开发者可以通过 `sitemap.json` 配置来设置小程序页面是否允许被微信索引。当开发者允许微信索引时，微信会通过爬虫的方式为小程序的页面内容建立索引。用户的搜索词触发该索引时，小程序的页面将可能展示在搜索结果中。

```
{
    "desc": "关于本文件的更多信息，请参考文档 https://developers.weixin.qq.com/miniprogram/dev/framework/sitemap.html",
    "rules": [{
    "action": "allow",
    "page": "*"
    }]
}
```

## 二、配置说明

### 2.1 默认情况

```
{
    "rules": [{
    "action": "allow",
    "page": "*"
    }]
}
```
表示所有页面都会被微信索引（默认情况）

### 2.2 页面不被索引，其余页面允许被索引

```
{
    "rules": [{
    "action": "disallow",
    "page": "pages/my/my"
    }]
}
``` 
表示 `pages/my/my` 页面不被索引，其余页面允许被索引。

### 2.3 页面被索引，其余页面不被索引

```
{
    "rules": [{
    "action": "allow",
    "page": "pages/my/my"
    },
    {
    "action": "disallow",
    "page": "*"
    }
    ]
}
```
表示 `pages/my/my` 页面允许被索引，其余页面不允许被索引。




## 三、注意事项

1、没有 `sitemap.json`，则默认所有页面都能被索引；
2、{"action": "allow","page": "*"} 是优先级最低的默认规则，未显式指明 "disallow" 的都默认被索引；