## 1、介绍

`bxcodec/faker` 是 Go 语言中造数据利器的第三方库，它非常适用于开发过程中需要填充数据库、创建测试数据。可以生成各种类型的假数据，包括字符串、数字、日期、布尔值、地址、姓名、邮箱、手机号、身份证号等。

## 2、安装

```
go get github.com/bxcodec/faker/v3
```

## 3、引入包

```
import "github.com/bxcodec/faker/v3"
```

## 4、生成假数据

### 4.1 使用 faker 提供的各种函数生成假数据

```
package main

import (
	"fmt"

	"github.com/bxcodec/faker/v3"
)

func main() {

	chineseName := faker.ChineseName()
	fmt.Println("生成的中文姓名：", chineseName)

	name := faker.Name()
	fmt.Println("生成的姓名：", name)

	email := faker.Email()
	fmt.Println("生成的邮箱地址：", email)

	pwd := faker.Password()
	fmt.Println("生成的密码：", pwd)

	phoneNumber := faker.Phonenumber()
	fmt.Println("生成的电话号码：", phoneNumber)

	gender := faker.Gender()
	fmt.Println("生成的性别：", gender)

	year := faker.YearString()
	fmt.Println("生成的年份：", year)

	ipv4 := faker.IPv4()
	fmt.Println("生成IPv4：", ipv4)

	ipv6 := faker.IPv6()
	fmt.Println("生成IPv6：", ipv6)

	macAddress := faker.MacAddress()
	fmt.Println("生成mac地址：", macAddress)

	domainName := faker.DomainName()
	fmt.Println("生成域名：", domainName)

	url := faker.URL()
	fmt.Println("生成URL：", url)

	uuiddigit := faker.UUIDDigit()
	fmt.Println("生成UUID：", uuiddigit)

}

// OUTPUT:
// 生成的中文姓名： 公鸿德
// 生成的姓名： Lady Audie Reynolds
// 生成的邮箱地址： KpmPJgl@LGdKqxc.com
// 生成的密码： lpvaGDZQBAxZUgkPsSQuajHnskjHFsceadUNtsscEpvsbUgASe
// 生成的电话号码： 723-956-1041
// 生成的性别： Prefer to skip
// 生成的年份： 1986
// 生成IPv4： 53.220.70.183
// 生成IPv6： 64c0:1ca7:5a29:6207:6ab1:85b0:5b12:9c0d
// 生成mac地址： 7a:64:29:77:11:34
// 生成域名： GMqvyDy.info
// 生成URL： https://www.lAXxXgd.net/
// 生成UUID： 8369bbc3e1944a1aa871dc51228ed1e
```

### 4.2 创建一个结构体并使用 faker 填充数据

```
package main

import (
	"fmt"

	"github.com/bxcodec/faker/v3"
)

type Person struct {
	Name        string
	ChineseName string
	Age         int
	Gender      string
	Email       string
}

type _Person struct {
	Name        string `faker:"name"`
	ChineseName string `faker:"chinese_name"`
	Age         int
	Gender      string `faker:"gender"`
	Email       string `faker:"email"`
}

func main() {

	var person Person
	var _person _Person

	if err := faker.FakeData(&person); err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("使用 faker 生成数据：%+v\n", person)

	if err := faker.FakeData(&_person); err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("使用 faker 生成数据：%+v\n", _person)
}

// 结构体不加标签的情况下，生成的是随机数据
//使用 faker 生成数据：{Name:RaBpPTROBtlhQniRnZAOEiZCf ChineseName:kjMwmcaZXiEJujSWwgfCrbbJi Age:58 Gender:sFChOFrrNVBMLChsTZHVHrMnJ Email:pscrGGDfMxuRcAjWQgkMlirls}
// 结构体加标签的情况下，生成的是符合标签要求的数据
// 使用 faker 生成数据：{Name:Mrs. Caleigh Miller ChineseName:欧阳鸿朗 Age:11 Gender:Male Email:cEFZfXy@xNgWDUE.net}
```