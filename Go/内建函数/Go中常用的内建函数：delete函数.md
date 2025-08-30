
在 Go 语言中，`delete` 函数是用来从 map 中删除键值对的，如果你想从一个 map 中删除一个特定的键及其对应的值，那么你就可以使用 `delete` 函数。这个函数是内置的，在使用的时候不需要导入任何包。

## 一、语法介绍

```go
delete(map, key)
```
内建函数 `delete` 专用于删除一个 map 中的某个键值对。它接受两个参数，第一个参数是想要删除元素的 map，而第二个参数则是能够代表要删除的那个键值对的键。

## 二、示例说明

###  2.1 简单示例

```go
package main
 
import "fmt"
 
func main() {
    subjects := map[string]int{
        "Math": 95,
        "English":125,
        "Chinese": 135,
    }
 
    // 打印初始的 map
    fmt.Println("Initial map:", subjects)
 
    // 删除键 "English"
    delete(subjects, "English")
 
    // 打印删除后的 map
    fmt.Println("Map after deletion:", subjects)
}
```

输出结果：

```
Initial map: map[Math:95 English:125 Chinese:135]
Map after deletion: map[Math:95 Chinese:135]
```

### 2.2 使用 Mutex 保护 map 的并发访问

```go
package main
 
import (
    "fmt"
    "sync"
)
 
func main() {
    var mu sync.Mutex // 创建一个 Mutex 锁

    subjects := map[string]int{
        "Math": 95,
        "English":125,
        "Chinese": 135,
    }
 
    mu.Lock()                   // 在修改前锁定 map
    delete(subjects, "English") // 安全地删除键 "English"
    mu.Unlock()                 // 修改完成后解锁 map
 
    fmt.Println("Map after deletion:", subjects)
}

```

这样，即使在并发环境下，我们也可以安全地修改 map。

## 三、注意事项

1、使用 `delete` 函数删除 map 中的元素并不会返回被删除的键值，它仅仅从 map 中删除指定的键及其对应的值；

2、如果删除的键在 map 中不存在，`delete` 函数只是简单的不做任何操作，也不会返回错误。这意味着我们不会收到任何错误或者异常信息，但如果尝试访问这个不存在的键，那么你会得到一个 nil；

3、在并发情况下访问，如果多个 `goroutine` 同时修改同一个 map，可能会导致运行时 panic。为了避免这种情况，你应该使用 `sync.Mutex` 或者其他同步机制来保护对 map 的访问；



