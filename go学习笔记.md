# go

------

## 目录



[toc]

-----

## 1 、常用快捷键

ctrl+D | 复制行
ctrl+X | 删除行
ctrl+alt+i | 对齐
alt+shift+F10 | 运行
ctrl+alt+a

------

## 2 、问题

### 1、println  printf  print 的区别

[区别][https://www.jianshu.com/p/9adbe60cb503]
有%d之类的输出的时候只能用printf

```go
	fmt.Println()//输出任意类型数据，并换行
	fmt.Print()  //输出任意类型数据，不换行
	fmt.Printf()//格式化输出数据，不换行
```



### 2、

```go
package main
import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```

### 3、切片的长度和容量

```go
  //切片的声明方式
	//1.nil方式
	var slice []int;
	//2.make方式
	var slice_01 = make([]int,3);相当于长度和容量都是3;
    或者指定长度和容量var slice_01 = make([]int,3,5);
	//3.
	var slice_02 = []int{1,2,3};
	//4.m<=n
	var slice_03 = [m:n];
 
    切片的长度就是len比较简单也是我们明面能够看得到的，容量和长度有些不同；着重讲解
    var slice = make([]int,0,5);
	slice = append(slice, 1);
	slice = append(slice, 2);
	slice = append(slice, 3);
	fmt.Printf("len = %d and cap = %d",len(slice),cap(slice))
    len = 3 and cap = 5 长度没有超过容量，继续使用当前容量即容量没有变化
    继续追加
    slice = append(slice, 4);
	slice = append(slice, 5);
	fmt.Printf("len = %d and cap = %d",len(slice),cap(slice))
    len = 5 and cap = 5
    长度没有超过容量，len=cap了，还是没有超出容量所以继续使用当前容量即容量没有变化
    slice = append(slice, 6);
    fmt.Printf("len = %d and cap = %d",len(slice),cap(slice))
    长度要超出容量了，需要扩容，扩容的容量大小是当前长度*2,即cap = len * 2;
    len = 6 and cap = 10
    后续继续扩大长度的机制：
    如果当前切片的长度小于 1024 就会将容量翻倍；
    如果当前切片的长度大于 1024 就会每次增加 25% 的容量，直到新容量大于期望容量；    

————————————————
版权声明：本文为CSDN博主「一只勤奋的代码狗」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/gaoxuaiguoyi/article/details/108393829
```

### 4、函数闭包（看不懂啊啊）



---------

## 3、笔记

### 1、函数

```go
func swap(x, y string) (string, string) { 
	return y, x
}  

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```



先命名函数变量类型，然后也要定义返回值类型
第一个括号里面的是函数变量，第二个括号里面是返回值

### 2、：=

在函数中，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明。函数外的每个语句都必须以关键字开始（`var`, `func` 等等），因此 `:=` 结构不能在函数外使用。常量也不能用`：= `

### 3、%T   %v   %q    %g    %s     %d

​    fmt.Printf("Type: **%T **Value:**%v **\n", ToBe, ToBe)
​	fmt.Printf("Type: **%T **Value: **%v**\n", MaxInt, MaxInt)
​	fmt.Printf("%v %v %v **%q**\n", i, f, b, s)

### 4、数据类型

Go 的基本类型有

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
    // 表示一个 Unicode 码点

float32 float64

complex64 complex128
```

本例展示了几种类型的变量。 同导入语句一样，变量声明也可以“分组”成一个语法块。

`int`, `uint` 和 `uintptr` 在 32 位系统上通常为 32 位宽，在 64 位系统上则为 64 位宽。 当你需要一个整数值时应使用 `int` 类型，除非你有特殊的理由使用固定大小或无符号的整数类型。

### 3、包 函数 调用

一个文件夹就是一个包package ，函数名第一个字母必须大写才能在包外调用，小写的只能在一个包里面调用

### 4、切片的创建

```go
var s []int
s := []int {2, 3, 4, 5}
s := make([]int, 4, 5) //(type, len, cap)
```

切片可以包含任何类型 ，甚至是包括其他的切片

```go
board := []  []string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}
```

### 5、go的输入和输出

[输入输出][https://blog.csdn.net/weixin_43688691/article/details/101771912]

### 6、for循环的range可遍历切片

使用for

## 4、推荐书籍和网站

go语言四十二章经     框架gin

## 5.应用

```go 
package resource

import (
	"fmt"
    "github.com/360EntSecGroup-Skylar/excelize"
    "github.com/go-redis/redis"
    "github.com/tidwall/gjson"
    "io/iotil"
    "net/http"
    "strings"
    "time"
)

type ResourcesHandler interface {
    InitKey() error
    InitResource() error
    WriteExcel() error
}

type Item struct {
    ParentId string
    Id       string
    Value    string
    Name     string
}

type RedisResource struct {
    client   *redis.Client
    keys     []string
    items    []Item
}

//连接API
func (r *RedisResource) InitResource() error {
    //1.登陆
    for _ , v := range r.keys {
        command := '{
        "translate": 1,
        "where": [{
            "terms": [{
                "field": "resource_id",
                "operator": "eq",
                "value": "item2replace"
            }]
        }]
    }'
    command = strings.Replace(command,"item2replace", v, -1)
    request, err := http.NewRequest("POST", "http://192.168.3.40/api/v2/cmdb/resources/items", strings.Newreader(command))
    if err != nil {
        return err
    }
    
    request.Header.Add("Cookie", "MODE=0;X_GU_SID=XSS_WnihGeR4nzlxUHwpFvS7TC4Goea63Ke1w0XjNgo687B")
    request.Header.Add("Host", "192.268.3.40")
    request.Header.Add("Origin", "http:192.168.3.40")
    request.Header.Add("Referer","")
    
    client := &http.Client{}
    res, err := client.Do(request)
    
    cookies := 
    }
}
```

