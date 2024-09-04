## 全局配置 app.json

```
{
  "pages": [
    "pages/index/index"
  ],
  "window": {
    "navigationBarTextStyle": "black",  
    "navigationStyle": "custom"
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json",
  "lazyCodeLoading": "requiredComponents"
}
```

全局配置相关信息可参考官方文档：https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html

### 全局配置 /pages 配置

```
  "pages": [
    "pages/index/index", // 新建项目默认的第一个页面
    "pages/list/list",
    "pages/cate/cate",
    "pages/my/my"
  ]
```

`pages` 字段：用来指定小程序由哪个页面组成，用于让小程序知道由哪个页面组成以及页面定义在哪个目录，每一项都对应一个页面的路径信息。

 > **注意事项：**

 1、页面路由不需要写文件后缀，框架会自动去寻找对应位置的四个文件进行处理；

 2、小程序中新增 / 删除页面时，都需要对全局配置文件 app.json 中的 pages 数组进行修改；

 3、未指定 `entryPagePath` 时，数组的第一项代表小程序的首页（初始化页面）；
 
 ```
  "entryPagePath": "pages/index/index",
  "pages": [
    "pages/index/index",
    "pages/list/list",
    "pages/cate/cate",
    "pages/my/my"
  ],
 ```

### 全局配置 /window 配置

```
"window": {
    "navigationBarTextStyle": "black",  
    "navigationStyle": "custom"
},
```

`window` 字段：用于配置小程序的状态栏、导航条、标题、窗口背景色等。

![mnp-13.png](../images/miniProgram/mnp-13.png)

### 全局配置 /tabBar 配置

`tabBar` 字段：定义小程序顶部、底部的 tab 栏，用以实现页面之间的快速切换。可以通过 `tabBar` 配置项指定 `tab` 栏的表现以及 `tab` 切换时显示的对应页面。

![mnp-14.png](../images/miniProgram/mnp-14.png)

 > **注意事项：**

 - tab 按数组的顺序排序，list 配置最少 2 个，最多 5 个。