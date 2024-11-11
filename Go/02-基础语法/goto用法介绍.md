## 一、概述

`goto` 在 Go 语言中用来实现非正常的流程控制，即从程序的一处跳转到另外一处。因为其会导致程序变得难以维护，所以并不推荐或者避免使用 `goto`，推荐使用条件语句或者循环等控制语法。

## 二、语法规则

`goto` 语句通过定义标签（label）进行跳转，标签是一个以冒号（：）结尾的标识符，它表示程序中的某个位置，通过 `goto` 语句指定跳转到该标签的位置。

## 三、示例说明


### 3.1 示例一

**注意：**该示例会无限输出，终止循环请按 `Ctrl + c` 或 `Cmd + c`
```
func main() {
	// 定义一个标签 Start
Start:
	fmt.Println("Start of the program")
	// 使用 goto 语句跳转到标签 end
	goto End
	fmt.Println("unreachable")

End:
	fmt.Println("End of the program")
	goto Start

}
```

### 3.2 示例二

```
func main() {
	i := 0
	i++
	if i > 5 {
		goto Here // 跳转到标签Here
	}
Here: // 标签
	fmt.Println(i)
}
```
输出结果：1

```
func main() {
	i := 0
	i++
	if i < 5 {
		goto Here // 跳转到标签Here
	}
Here: // 标签
	fmt.Println(i)
}
```

执行 goto 时，跳转到 Here，属于`脱裤子放屁，多此一举，哈哈！`，不执行goto时，会按照顺序执行。

### 3.3 示例三

**注意：**该示例会无限输出，终止循环请按 `Ctrl + c` 或 `Cmd + c`

```
func main() {
Outer:
	for i := 0; i < 5; i++ {
		for j := 0; j < 5; j++ {
			if j == 3 {
				goto Outer // 跳出外层循环
			}
			fmt.Printf("i = %d, j = %d\n", i, j)
		}
	}
}
```

## 四、总结

建议尽量避免使用 `goto`，并优先考虑使用 if、switch、for等条件、循环控制语句构建程序逻辑。