
`YAML`，Kubernetes 世界里的标准工作语言，一起了解下 `YAML` 是什么？为什么要有 `YAML`? 它是个什么样子并且怎么使用它。

## YAML 是什么？

`YAML` 官网：https://yaml.org/ (对语言规范有详细的介绍)，引用官网一段话：`YAML is a human-friendly data serializationlanguage for all programming languages.`。它是一种更适合人类可读的数据序列化格式，让我们可以在不同的编程语言下轻松编写和解析。 

## 使用场景

- 配置文件（如 Docker Compose、 Kubernetes、 Ansible）;
- 跨语言数据交换；
- 结构化数据存储（如： OpenAPI、 Swagger 的 API 规范）；

## 支持的数据类型

- 支持整数、浮点数、布尔、字符串；
- 支持序列（列表、数组）；
- 映射（字典、哈希表）；
- 多行字符串（| 保留换行符、> 折叠为单行）；

另外，我们需要知道，`YAML` 是 `JSON` 的超集，也就是说任何合法的 `JSON` 文档也都是 `YAML` 文档。与 `JSON` 文档比起来，显然可读性比较强，语法更简单些。

## 基本语法

- 使用空白与缩进表示层次，可以不使用花括号和方括号；
- 使用 `#` 注释；
- 数组（列表）是使用 `-` 开头的清单形式；
- 表示对象的 `:` 和表示数组的 `-` 后面必须要用空格；
- 使用 `---` 在一个文件里分隔多个 `YAML` 对象；

## 简单示例

```yaml
# 数组（列表）
OS:
 - Linux
 - MacOS
 - Windows
 - HarmonyOS
# 键值对
name: 巴莫
age: 18
is_student: false

# 对象（字典）
address:
 province: 火星省
 city: 巴拉市
 district: 有水区
```

对应的 `JSON` 格式如下：

```json
{
    "OS": ["Linux","MacOS","Windows","HarmonyOS"]
}

{
    "name": "巴莫",
    "age": 18,
    "is_student": false
}

{
    "address": {
        "province": "火星省",
        "city": "巴拉市",
        "district": "有水区"
    }
}

```
可以看出来 `YAML` 和 `JSON` 对比，有一个明显的变化就是 `YAML` 里面的 key 不需要使用双引号，看起来很简洁干练。

再举一个 `Kubernetes` 里面的示例再看看：

```
Kubernetes:
  master:
    - apiserver: running
    - etcd: running
  node:
    - kubelet: running
    - kube-proxy: down
    - container-runtime: [docker, containerd, cri-o]
```

这个结构表示：顶层对象是 `Kubernetes`，包含两个主要部分：`master` 和 `node`；

1、**`master` 部分是一个数组，包含两个对象：**

- `apiserver` 的状态是 "running"；
- `etcd` 的状态是 "running"；

2、**`node` 部分是一个数组，包含三个元素：**

- `kubelet` 的状态是 "running"；
- `kube-proxy` 的状态是 "down"（表示不正常）；
- `container-runtime` 是一个数组，列出了三种可能的容器运行时选项：`docker`、`containerd` 和 `cri-o`；

这种结构很好地展示了 `Kubernetes` 集群中主节点和工作节点的不同组件及其状态。在实际使用中，这种格式可以用来记录或报告集群的健康状态。



