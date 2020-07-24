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
        value:=val
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

