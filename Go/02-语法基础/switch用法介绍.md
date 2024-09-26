## 一、概述

Go 语言中的 `switch` 语句是一种控制流语句，基于不同条件下执行不同的逻辑处理。与其他编程语言不同的是，Go 中不限定使用特定类型的表达式，接受任意形式的表达式，如：数值、字符串、布尔值等。

## 二、语法规则

```
switch expr {   // expr 表达式可以是任意类型
  case v1: // 当expr==v1时执行的代码 
      ...
  case v2:
      ...
  case v3:
      fallthrough
  case v4, v5, v6:    // 多个可能符合条件的值
      ...
  default:  // 默认值，expr的值与上面任何一个case都不匹配时执行

      ...
}
```

- `switch` 后面的表达式不需要括号；
- `case` 语句块执行结束后，自动退出整个 `switch` 语句块，不需要显式声明 `break`;
- `case` 语句块执行结束后，如果需要向下继续执行，使用关键字 `fallthrough`，类似java中的switch语句块中不加 `break` 一样;

## 三、示例说明

```
func main() {
	fruit := "apple"
	switchofgeneral(fruit)

	switchofexpression()

	switchofdefault()

	switchofomitexpression()

	switchofmorecase()

	switchoffallthrough()

	switchoftypeassert()
}
```
执行 `go run main.go`

### 3.1 常规表达式

```
func switchofgeneral(fruit string) {
	switch fruit {
	case "apple":
		fmt.Printf("fruit: %s\n", fruit)
	case "banana":
		fmt.Printf("fruit: %s\n", fruit)
	case "orange":
		fmt.Println("orange")
	}
}
```
输出：
```
fruit: apple
```

### 3.2 运算表达式

```
func switchofexpression() {
	n := 4
	switch n * 2 {
	case 8:
		fmt.Println("8")
	case 10:
		fmt.Println("10")
	}
}
```

输出：
```
8
```

### 3.3 default

```
func switchofdefault() {
	n := 1024
	switch n * 2 {
	case 0:
		fmt.Println("0")
	case 1:
		fmt.Println("1")
	case 10:
		fmt.Println("10")
	default:
		fmt.Println("1024")
	}
}
```
输出：
```
1024
```

### 3.4 省略 expr 表达式

```
func switchofomitexpression() {
	n := 1024
	switch {
	case n < 1024:
		fmt.Println("n < 1024")
	case n > 1024:
		fmt.Println("n > 1024")
	case n == 1024:
		fmt.Println("n==1024")
	default:
		fmt.Println("无结果")
	}

}
```

输出：
```
n==1024
```

### 3.5 多个 case 值

```
func switchofmorecase() {
	n := 1024
	switch n {
	case 1023, 1024:
		fmt.Println("n <= 1024")
	case 1025:
		fmt.Println("n > 1024")
	default:
		fmt.Println("无结果")
	}
}
```

输出：
```
n <= 1024
```

### 3.6 fallthrough

```
func switchoffallthrough() {
	n := 1024
	switch {
	case n < 1024:
		fmt.Println("n < 1024")
		fallthrough // 继续向下执行
	case n > 1024:
		fmt.Println("n > 1024")
		fallthrough // 继续向下执行
	case n == 1024:
		fmt.Println("n == 1024")
		fallthrough // 继续向下执行
	default:
		fmt.Println("无结果")
	}
}
```
输出：
```
n == 1024
无结果
```

### 3.7 类型断言

```
func switchoftypeassert() {
	var n any = 1024
	switch n.(type) {

	case int:
		println("n is a int")
	case float64:
		println("n is a float64")
	case bool:
		println("n is a bool")
	case string:
		println("n is a string")
	default:
		println("n is invalid")
	}
}
```
输出：
```
n is a int
```

## 四、总结

Go 语言中的 `switch` 语句使用说明：

1、一旦某个 case的值与 `expr` 表达式匹配执行后，`case` 分支默认会自动结束并退出 `switch` 语句，无需 `break` 语句。如果想继续执行下一个 `case` 语句，可以使用关键字 `fallthrough`，继续则依次类推即可；

2、`case` 后面可以写多个值，用逗号隔开；

3、`switch` 语句后面可以没有 `expr` 表达式，此时相当于是 `switch true`；

4、`switch` 可以用于类型断言，可以用于检查并处理接口类型的值；