---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Go语言中文网面试题 1-30"
subtitle: ""
summary: "Go语言面试题"
authors: [""]
tags: ["Go"]
categories: ["interview"]
date: 2019-12-01T19:15:17+08:00
lastmod: 2019-12-01T19:15:17+08:00
featured: false
draft: false
toc: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "featured"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []

---

## Day 1. panic 和 defer 顺序

问：下面这段代码的打印顺序

```go
package main

import (
    "fmt"
)

func main() {
    defer_call()
}

func defer_call() {
    defer func() { fmt.Println("打印前") }()
    defer func() { fmt.Println("打印中") }()
    defer func() { fmt.Println("打印后") }()

    panic("触发异常")
}
```

输出：

```go
打印后
打印中
打印前
panic:触发异常
```

> `defer` 的执行顺序是先进后出，当初`panic`语句时，会先按照`defer`的后进先出的顺序执行，最后才执行`panic`

> 若把panic 移动到defer 上 ，则只有 panic("触发异常") 。

## Day 2. for range 创建元素副本，非引用

问：代码的输出

```c++
package main

import "fmt"

func main() {
	slice := []int{0, 1, 2, 3}
	m := make(map[int]*int)

	for key, val := range slice {
		m[key] = &val
	}

	for k, v := range m {
		fmt.Println(k, "->", *v)
	}
}
```

```go
0 -> 3
1 -> 3
2 -> 3
3 -> 3
```

> for range 循环的时候会**创建每个元素的副本，而不是元素的引用**，所以 m[key] = &val 取的都是变量 val 的地址，所以最后 map 中的所有元素的值都是变量 val 的地址，因为最后 val 被赋值为3，所有输出都是3.

```go
package main

import "fmt"

func main() {
	slice := []int{0, 1, 2, 3}

	m := make(map[int]*int)

	for key, val := range slice {
		value := val
		m[key] = &value
		
	}

	for k, v := range m {
		fmt.Println(k, "->", *v)
	}
}

```

```go
2 -> 2
3 -> 3
0 -> 0
1 -> 1
```

## Day 3. append, 命名返回值, new和make区别

问1：代码输出

```go
package main

import "fmt"

func main() {
	s1 := make([]int, 5)
	s1 = append(s1, 1, 2, 3, 4)

	s2 := make([]int, 0)
	s2 = append(s2, 1, 2, 3, 4)

	s3 := make([]int, 2)
	s3 = append(s3, 1, 2, 3, 4)

	fmt.Println("s1: ", s1)
	fmt.Println("s2: ", s2)
	fmt.Println("s3: ", s3)
}
```

```go
s1:  [0 0 0 0 0 1 2 3 4]
s2:  [1 2 3 4]
s3:  [0 0 1 2 3 4]
```

****

问2：代码缺陷错误

```go
func add(x,y int)(sum int, error){
	return x+y,nil
}
```

> 在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名。
>
> 如果有多个返回值必须加上括号()；
>
> 如果只有一个返回值且命名也必须加上括号()。
>
> 这里的第一个返回值有命名 sum，第二个没有命名，所以错误。

```go
//对
func add(x,y int)(sum int, err error){
	return x+y,nil
}
```

------

问3: make 和 new 的区别

new(T) 和 make(T, args ) 是 Go 语言内建函数，用来分配内存，但适用的类型不同。

new(T) 会为 T 类型的新值分配已置零的内存空间，并返回地址（指针），即类型为 *T 的值。换句话说就是，返回一个指针，该指针指向新分配的、类型为 T 的零值。适用于值类型，如数组、结构体等。

make(T,args) 返回初始化之后的 T 类型的值，这个值并不是 T 类型的零值，也不是指针 *T，是经过初始化之后的 T 的引用。make() 只适用于 slice、map 和 channel.

***

## Day 4. append, :=

问1：能否通过编译

```go
package main
import "fmt"
//否
func main() {
	s1 := new([]int)
	s1 := append(s1, 2)
	fmt.Println(s1)
}
```

不能对指针执行 append 操作,可以使用 make() 初始化之后再用。同样的，map 和 channel建议使用 make() 或字面量的方式初始化，不要用 new() 

***

问2：能否通过编译

```go
package main

import "fmt"
//否
func main() {
	s1 := []int{1, 3, 2}
	s2 := []int{4, 5}
	s1 = append(s1, s2)
	fmt.Println(s1)
}
```

不能，append() 的第二个参数不能直接使用 slice，需使用 … 操作符，将一个切片追加到另一个切片上：append(s1,s2…)。或者直接跟上元素，形如：append(s1,1,2,3)

```go
package main
import "fmt"

func main() {
	
	s1 := []int{1, 3, 2}
	s2 := []int{4, 5}
	s1 = append(s1, s2...)
	fmt.Println(s1)
}
```

```go
[1 3 2 4 5]
```

****

问3：能否通过编译

```go
var (
	size := 1024
	max_size := size*2
)

func main() 
	fmt.Println(size,max_size)
}
```

否，变量声明的简短模式，x := 100。但这种声明方式有限制：

1. 必须使用显示初始化；
2. 不能提供数据类型，编译器会自动推导；
3. 只能在函数内部使用简短模式；

## Day 5. 结构体比较

能否通过编译

```go
package main

import "fmt"

func main() {
	st1 := struct {
		age  int
		name string
	}{age: 3, name: "qq"}
	st2 := struct {
		age  int
		name string
	}{age: 3, name: "qq"}

    //成员属性顺序不一样，也不能比较
	st3 := struct {
		name string
		age  int
	}{age: 3, name: "qq"}

	fmt.Println(st1 == st2)
    
	//fmt.Println(st3 == st1)编译错误

	sm1 := struct {
		age int
		n   map[string]string
	}{age: 5, n: map[string]string{"sd": "sf"}}
	sm2 := struct {
		age int
		n   map[string]string
	}{age: 5, n: map[string]string{"sd": "sf"}}

	//fmt.Println(sm2 == sm1) 编译错误
}
```

1. 结构体只能比较是否相等，但是不能比较大小。
2. 相同类型的结构体才能够进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关，sn3 与 [sn1](http://mp.weixin.qq.com/s?__biz=MzI2MDA1MTcxMg==&mid=2648466814&idx=1&sn=611ad5be36e7c886126f67da3f11af0e&chksm=f2474311c530ca070cdaa791fbe488b9ecd66667f7d08ee78068c5fbb33db1114c7bdd4a625c&scene=21#wechat_redirect) 就是不同的结构体；
3. 如果 struct 的所有成员都可以比较，则该 struct 就可以**通过 == 或 != 进行比较**是否相等，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等；

**可比较**： bool、数值型、字符、指针、数组等

**不可比较**：切片、map、函数等。

## Day 6. 指针，类型别名与定义，值传递

问1.通过指针变量 p 访问其成员变量 name，有哪几种方式？

- A.p.name
- B.(&p).name
- C.(*p).name
- D.p->name

答：AC

***

问2：能否通过编译

```go
package main

import "fmt"

type myInt1 int
type myInt2 = int

func main() {
	var i int = 0
	var i1 myInt1 = i
	var i2 myInt2 = i
	fmt.Println(i1, i2)
}
```

```go
.\day-6.go:10:6: cannot use i (type int) as type myInt1 in assignment
```

类型别名与类型定义的区别。

第 5 行代码是基于类型 int 创建了新类型 MyInt1

第 6 行代码是创建了 int 的类型别名 MyInt2，注意类型别名的定义时 = 。

所以，第 10 行代码相当于是将 int 类型的变量赋值给 MyInt1 类型的变量，Go 是强类型语言，编译当然不通过；而 MyInt2 只是 int 的别名，本质上还是 int，可以赋值。

第 10 行代码的赋值可以使用强制类型转化 var i1 MyInt1 = MyInt1(i).

***

问3：输出是什么

```go
package main

import "fmt"

func main() {
	a := []int{7, 8, 9}
	fmt.Printf("%+v\n", a)
	ap(a)
	fmt.Printf("%+v\n", a)
	app(a)
	fmt.Printf("%+v\n", a)

}

func ap(a []int) {
	a = append(a, 10)
}

func app(a []int) {
	a[0] = 1
}
```

```c++
[7 8 9]
[7 8 9]
[1 8 9]
```

 append 导致底层数组重新分配内存了,创建了新切片，ap 中的 a 这个slice 的底层数组和外面的不是一个，并没有改变外面的。app会修改底层数组内容，会改变

## Day 7. 字符串拼接，iota

问1： 关于字符串连接，下面语法正确的是？

- A. str := 'abc' + '123'
- B. str := "abc" + "123"
- C. str := '123' + "abc"
- D. fmt.Sprintf("abc%d", 123)

答：BD。知识点：字符串连接。除了以上两种连接方式，还有 strings.Join()，buffer.WriteString()等。

***

问2：输出什么

```go
package main
import "fmt"

const (
	x = iota
	_
	y
	z = "zz"
	k
	p = iota
)

func main() {
	fmt.Println(x, y, z, k, p)
}
```

```go
0 2 zz zz 5
```

[iota详细](https://www.cnblogs.com/zsy/p/5370052.html)

**1iota是golang语言的常量计数器,只能在常量的表达式中使用。**

**每次 const 出现时，都会让 iota 初始化为0.【自增长】**

```go
const a = iota // a=0
const (
 	b = iota     //b=0
 	c           / /c=1
)
```

**可以使用下划线跳过不想要的值**

**中间插队**

```go
const (
  i = iota
  j = 3.14
  k = iota
  l
)
//那么打印出来的结果是 i=0,j=3.14,k=2,l=3
```



