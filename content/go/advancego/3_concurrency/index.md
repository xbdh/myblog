---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "week3 Concurrency Goroutine开启"
subtitle: ""
summary: "Goroutine"
authors: []
tags: ["go"]
categories: ["go","go进阶训练营"]
date: 2020-12-03T12:36:48+08:00
lastmod: 2020-12-03T12:36:48+08:00
featured: false
draft: false
toc: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## 一. 掌控Goroutine

### 1. 并发与并行

#### parallelism



#### concurrency

### 2. Keep yourself busy or do the work yourself

> 不要做一个旁观者，能自己做的事情自己做。

如果你的 goroutine 在从另一个 goroutine 获得结果之前无法继续运行下去，通常情况下，你自己去做这项工作比委托它( go func() )更简单。

消除了从 goroutine开启到其返回结果所需的大量状态跟踪和 chan 操作。

#### **一个不正确的使用例子**

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"runtime"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Hello, GopherCon SG")
	})
	go func() {
		if err := http.ListenAndServe(":8080", nil); err != nil {
			log.Fatal(err)
		}
	}()

    // 方法一：
    // main 一直阻塞
    for {
	}
    
    // 方法二：
    // 一种解决方式，但并未根本解决问题
	for {
		runtime.Gosched()
	}
    
    // 方法三：
    // 空的 select 语句将永远阻塞
    select {}
}
```

三种方法都治标不治本

#### 正确的解决方法

不在一个单独的goroutine 运行`http.ListenAndServe`，而是留给main goroutine

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Hello, GopherCon SG")
	})
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}
}
```

### 3. Never start a goroutine without knowing how and when it will stop

> 不要开启一个不能结束的goroutine，会导致内存泄漏
>
> 不要开启一个你不知到什么时候结束的goroutine

开启goroutine之前要问自己：

- **When will it terminate?**

- **What could prevent it from terminating?**

#### 内存泄漏的例子

main 退出，goroutine 还一直阻塞

```go
 // leak is a buggy function. It launches a goroutine that
 // blocks receiving from a channel. Nothing will ever be
 // sent on that channel and the channel is never closed so
 // that goroutine will be blocked forever.
 func leak() {
     ch := make(chan int)
 
     go func() {
        val := <-ch
        fmt.Println("We received a value:", val)
     }()
 }
```

#### 更复杂的内存泄漏例子

超时之后mian退出，goroutine阻塞

```go
// Example program showing a goroutine leak. It launches a
// goroutine that sends on a channel but sometimes there is
// no other goroutine available to receive.
package main

import (
	"context"
	"errors"
	"fmt"
	"log"
	"runtime"
	"time"
)

func main() {

	// Capture starting number of goroutines.
	startingGs := runtime.NumGoroutine()

	if err := process("gophers"); err != nil {
		log.Print(err)
	}

	// Hold the program from terminating for 1 second to see
	// if any goroutines created by process terminate.
	time.Sleep(time.Second)

	// Capture ending number of goroutines.
	endingGs := runtime.NumGoroutine()

	// Report the results.
	fmt.Println("========================================")
	fmt.Println("Number of goroutines before:", startingGs)
	fmt.Println("Number of goroutines after :", endingGs)
	fmt.Println("Number of goroutines leaked:", endingGs-startingGs)
}

// 包含了结果和错误
// result wraps the return values from search. It allows us
// to pass both values across a single channel.
type result struct {
	record string
	err    error
}

// process is the work for the program. It finds a record
// then prints it. It fails if it takes more than 100ms.
// 顺序调用产生的延迟可能是不可接受的 ,使用异步
func process(term string) error {

    // 100毫秒超时取消
	// Create a context that will be canceled in 100ms.
	ctx, cancel := context.WithTimeout(context.Background(), 100*time.Millisecond)
	defer cancel()

	// Make a channel for the goroutine to report its result.
	ch := make(chan result)

	// Launch a goroutine to find the record. Create a result
	// from the returned values to send through the channel.
	go func() {
		record, err := search(term)
		ch <- result{record, err}
	}()

	// Block waiting to either receive from the goroutine's
	// channel or for the context to be canceled.
	select {
	case <-ctx.Done():
		return errors.New("search canceled")
	case result := <-ch:
		if result.err != nil {
			return result.err
		}
		fmt.Println("Received:", result.record)
		return nil
	}
}

// 模拟查找过程，耗时200毫秒,
// 模拟长时间运行的操作，如数据库查询或 rpc 调用。
// search simulates a function that finds a record based
// on a search term. It takes 200ms to perform this work.
// 
func search(term string) (string, error) {
	time.Sleep(200 * time.Millisecond)
	return "some value", nil
}
```

output:

```go
2020/12/13 19:12:54 search canceled
========================================
Number of goroutines before: 1
Number of goroutines after : 2
Number of goroutines leaked: 1
```

解决：

```go
// Make a channel for the goroutine to report its result.
// Give it capacity so sending doesn't block.
 ch := make(chan result, 1)
```



#### 不知道何时结束的例子

这个简单的应用程序在两个不同的端口上提供 http 流量，端口8080用于应用程序流量，端口8001用于访问 /debug/pprof 端点。

无法知道goroutine的任何信息

```go
package main

import (
	"fmt"
	"net/http"
	_ "net/http/pprof"
)

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(resp http.ResponseWriter, req *http.Request) {
		fmt.Fprintln(resp, "Hello, QCon!")
	})
	go http.ListenAndServe("127.0.0.1:8001", http.DefaultServeMux) // debug
	http.ListenAndServe("0.0.0.0:8080", mux)                       // app traffic
}
```

**重构**

通过将 serveApp 和 serveDebug 处理程序分解为各自的函数，我们将它们与main.main 解耦，并确保 serveApp 和 serveDebug 将它们的并发性留给调用者。

如果 serveApp 返回，则 main.main 将返回导致程序关闭，只能靠类似 supervisor 进程管理来重新启动。

如果 serveDebug()退出，主程序的其余部分继续运行。由于 /debug 处理程序停止工作了，无法在应用程序中继续获取统计信息。

```go
func serveApp() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(resp http.ResponseWriter, req *http.Request) {
		fmt.Fprintln(resp, "Hello, QCon!")
	})
	http.ListenAndServe("0.0.0.0:8080", mux)
}

func serveDebug() {
	http.ListenAndServe("127.0.0.1:8001", http.DefaultServeMux)
}

func main() {
	go serveDebug()
	serveApp()
}
```

**再重构**

当goroutinue出错时， 负责自己的结束

```go
func serveApp() {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(resp http.ResponseWriter, req *http.Request) {
		fmt.Fprintln(resp, "Hello, QCon!")
	})
	if err := http.ListenAndServe("0.0.0.0:8080", mux); err != nil {
		log.Fatal(err)
	}
}

func serveDebug() {
	if err := http.ListenAndServe("127.0.0.1:8001", http.DefaultServeMux); err != nil {
		log.Fatal(err)
	}
}

func main() {
	go serveDebug()
	go serveApp()
	select {}
}
```

存在问题：

- ListenAndServer 返回 nil error，最终 main.main 无法退出。
- log.Fatal 调用了 os.Exit，会无条件终止程序；defers 不会被调用到。其他goroutine无法感知到这种退出，这使得为这些函数编写测试变得困难。

tips：

只在main，init中使用log.Fatal 

**再重构**

使用channel 搜集goroutine的信息，

```go
func serveApp() error {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(resp http.ResponseWriter, req *http.Request) {
		fmt.Fprintln(resp, "Hello, QCon!")
	})
	return http.ListenAndServe("0.0.0.0:8080", mux)
}

func serveDebug() error {
	return http.ListenAndServe("127.0.0.1:8001", http.DefaultServeMux)
}

func main() {
	done := make(chan error, 2)
	go func() {
		done <- serveDebug()
	}()
	go func() {
		done <- serveApp()
	}()

	for i := 0; i < cap(done); i++ {
		if err := <-done; err != nil {
			fmt.Println("error: %v", err)
		}
	}
}
```

**再重构**

一个goroutine退出，其他goroutine也都会退出

https://github.com/da440dil/go-workgroup

```go
func serve(addr string, handler http.Handler, stop <-chan struct{}) error {
	s := http.Server{
		Addr:    addr,
		Handler: handler,
	}

    // 这个 goroutine 我们可以控制退出，
    // 因为只要 stop 这个 channel close 或者是写入数据，这里就会退出
    // 同时因为调用了 s.Shutdown 调用之后，
    // http 这个函数启动的 http server 也会优雅退出
	go func() {
		<-stop // wait for stop signal
		s.Shutdown(context.Background())
	}()

	return s.ListenAndServe()
}

func serveApp(stop <-chan struct{}) error {
	mux := http.NewServeMux()
	mux.HandleFunc("/", func(resp http.ResponseWriter, req *http.Request) {
		fmt.Fprintln(resp, "Hello, QCon!")
	})
	return serve("0.0.0.0:8080", mux, stop)
}

func serveDebug(stop <-chan struct{}) error {
	return serve("127.0.0.1:8001", http.DefaultServeMux, stop)
}

func main() {
     // 用于监听服务退出
	done := make(chan error, 2)
     // 用于控制服务退出，传入同一个 stop，
     // 做到只要有一个服务退出了那么另外一个服务也会随之退出
	stop := make(chan struct{})
    
	go func() {
		done <- serveDebug(stop)
	}()
	go func() {
		done <- serveApp(stop)
	}()

    // stoped 用于判断当前 stop 的状态
	var stopped bool
    
    // 这里循环读取 done 这个 channel
    // 只要有一个退出了，我们就关闭 stop channel
	for i := 0; i < cap(done); i++ {
		if err := <-done; err != nil {
			fmt.Println("error: %v", err)
		}
		if !stopped {
			stopped = true
			close(stop)
		}
	}
}
```

存在问题：

- 但是我们其实并没有实现优雅退出，
- 在 `server` 方法中我们并没有处理 `panic` 的逻辑

#### incomplete work

服务端需要跟踪某些事件

```go
package main

import (
	"log"
	"net/http"
	"time"
)

// Tracker knows how to track events for the application.
type Tracker struct{}

// Event records an event to a database or stream.
func (t *Tracker) Event(data string) {
	 // Simulate network write latency.
     // 模拟网络延迟，把记录的东西存到数据库中
    time.Sleep(time.Millisecond)
	log.Println(data)
}

// App holds application state.
type App struct {
	track Tracker
}

// Handle represents an example handler for the web service.
func (a *App) Handle(w http.ResponseWriter, r *http.Request) {

	// Do some actual work.

	// Respond to the client.
	w.WriteHeader(http.StatusCreated)

	// Fire and Hope.
	// BUG: We are not managing this goroutine.
    // 开启异步goroutine，但无法其状态
	go a.track.Event("this event")
}

func main() {

	// Start a server.
	// Details not shown...
	var a App

	// Shut the server down.
	// Details not shown...
	_ = a
}
```

**重构1**

使用 sync.WaitGroup 来追踪每一个创建的 goroutine。

```go
package main

import (
	"log"
	"net/http"
	"sync"
	"time"
)

// Tracker knows how to track events for the application.
type Tracker struct {
	wg sync.WaitGroup
}

// Event starts tracking an event. It runs asynchronously to
// not block the caller. Be sure to call the Shutdown function
// before the program exits so all tracked events finish.
func (t *Tracker) Event(data string) {

	// Increment counter so Shutdown knows to wait for this event.
	t.wg.Add(1)

	// Track event in a goroutine so caller is not blocked.
	go func() {

		// Decrement counter to tell Shutdown this goroutine finished.
		defer t.wg.Done()

		time.Sleep(time.Millisecond) // Simulate network write latency.
		log.Println(data)
	}()
}

// Shutdown waits for all tracked events to finish processing.
func (t *Tracker) Shutdown() {
	t.wg.Wait()
}

// App holds application state.
type App struct {
	track Tracker
}

// Handle represents an example handler for the web service.
func (a *App) Handle(w http.ResponseWriter, r *http.Request) {

	// Do some actual work.

	// Respond to the client.
	w.WriteHeader(http.StatusCreated)

	// Track the event.
	a.track.Event("this event")
}

func main() {

	// Start a server.
	// Details not shown...
	var a App

	// Shut the server down.
	// Details not shown...

	// Wait for all event goroutines to finish.
    // 等待所有goroutine结束，等待时间过长
	a.track.Shutdown()
}

```

重构2

超时控制

```go
package main

import (
	"context"
	"errors"
	"log"
	"net/http"
	"sync"
	"time"
)

// Tracker knows how to track events for the application.
type Tracker struct {
	wg sync.WaitGroup
}

// Event starts tracking an event. It runs asynchronously to
// not block the caller. Be sure to call the Shutdown function
// before the program exits so all tracked events finish.
func (t *Tracker) Event(data string) {

	// Increment counter so Shutdown knows to wait for this event.
	t.wg.Add(1)

	// Track event in a goroutine so caller is not blocked.
	go func() {

		// Decrement counter to tell Shutdown this goroutine finished.
		defer t.wg.Done()

		time.Sleep(time.Millisecond) // Simulate network write latency.
		log.Println(data)
	}()
}

// Shutdown waits for all tracked events to finish processing
// or for the provided context to be canceled.
func (t *Tracker) Shutdown(ctx context.Context) error {

	// Create a channel to signal when the waitgroup is finished.
	ch := make(chan struct{})

	// Create a goroutine to wait for all other goroutines to
	// be done then close the channel to unblock the select.
	go func() {
		t.wg.Wait()
		close(ch)
	}()

	// Block this function from returning. Wait for either the
	// waitgroup to finish or the context to expire.
	select {
	case <-ch:
		return nil
	case <-ctx.Done():
		return errors.New("timeout")
	}
}

// App holds application state.
type App struct {
	track Tracker
}

// Handle represents an example handler for the web service.
func (a *App) Handle(w http.ResponseWriter, r *http.Request) {

	// Do some actual work.

	// Respond to the client.
	w.WriteHeader(http.StatusCreated)

	// Track the event.
	a.track.Event("this event")
}

func main() {

	// Start a server.
	// Details not shown...
	var a App

	// Shut the server down.
	// Details not shown...

	// Wait up to 5 seconds for all event goroutines to finish.
	const timeout = 5 * time.Second
	ctx, cancel := context.WithTimeout(context.Background(), timeout)
	defer cancel()

	err := a.track.Shutdown(ctx)
	if err != nil {
		log.Println(err)
	}
}

```

在Shutdown()内部开启了goroutine，并没有把goroutine 交给调用者开启



重构3

毛剑老师，待看

```go
package main

import (
	"context"
	"fmt"
	"time"
)

type Tracker struct {
	ch chan string
	stop chan struct{}
}

func NewTracker()*Tracker  {
	return &Tracker{
		ch:   make(chan string,10),
	}
}
func (t *Tracker) Event(ctx context.Context,data string)  error{
	select {
	case t.ch <- data:
		return nil

	case <-ctx.Done():
		return ctx.Err()
	}
}
func (t *Tracker) Run()  {
	for data:=range t.ch{
		time.Sleep(1*time.Second)
		fmt.Println(data)
	}
	t.stop<- struct{}{}
}

func (t* Tracker) Shutdown(ctx context.Context)  {
	
	close(t.ch)
	select {
	case <-t.stop:
	case <-ctx.Done():
		
	}
}

func main()  {
	tr:=NewTracker()
	go tr.Run()
	_=tr.Event(context.Background(),"test")
	_=tr.Event(context.Background(),"test")
	_=tr.Event(context.Background(),"test")
	ctx,cancle:=context.WithDeadline(context.Background(),time.Now().Add(5*time.Second))

	defer cancle()
	tr.Shutdown(ctx)
}
```

output：

```go
test
test
test
```







### 4. Leave concurrency to the caller

> 让调用者实现并发

API区别

```go
// ListDirectory returns the contents of dir.
func ListDirectory(dir string) ([]string, error)


// ListDirectory returns a channel over which
// directory entries will be published. When the list
// of entries is exhausted, the channel will be closed.
func ListDirectory(dir string) chan string
```

**方法一：**

将目录读取到一个 slice 中，然后返回整个切片，或者如果出现错误，则返回错误。同步调用

**问题：**

ListDirectory 的调用方会阻塞，直到读取所有目录条目。如果目录的很大，这可能需要很长时间，并且可能会分配大量内存来构建目录条目名称的 slice。

**方法二：**

ListDirectory 返回一个 chan string，将通过该 chan 传递目录。当通道关闭时，这表示不再有目录。由于在 ListDirectory 返回后发生通道的填充，ListDirectory 可能内部启动 goroutine 来填充通道。

**问题：**

通过使用一个关闭的通道作为不再需要处理的项目的信号，如果读取过程中途遇到了错误，ListDirectory 无法告诉调用者通过通道返回的项目集不完整。调用方无法区分读取完毕与发生错误之间的区别。这两种方法都会导致从 ListDirectory 返回的通道会立即关闭。

调用者必须继续从通道读取，直到它关闭，因为这是调用者知道开始填充通道的 goroutine 已经停止的唯一方法。这对 ListDirectory 的使用是一个严重的限制，调用者必须花时间从通道读取数据，即使它可能已经收到了它想要的答案。对于大中型目录，它可能在内存使用方面更为高效，但这种方法并不比原始的基于 slice 的方法快。



**正确做法：**

使用回调函数

```go
func ListDirectory(dir string, fn func(string))
```

官方`filepath.WalkDir`使用了这种方式

```go
// func Walk(root string, walkFn WalkFunc) error
// type WalkFunc func(path string, info os.FileInfo, err error) error


// +build !windows,!plan9

package main

import (
	"fmt"
	"io/ioutil"
	"os"
	"path/filepath"
)

func prepareTestDirTree(tree string) (string, error) {
	tmpDir, err := ioutil.TempDir("", "")
	if err != nil {
		return "", fmt.Errorf("error creating temp directory: %v\n", err)
	}

	err = os.MkdirAll(filepath.Join(tmpDir, tree), 0755)
	if err != nil {
		os.RemoveAll(tmpDir)
		return "", err
	}

	return tmpDir, nil
}

func main() {
	tmpDir, err := prepareTestDirTree("dir/to/walk/skip")
	if err != nil {
		fmt.Printf("unable to create test dir tree: %v\n", err)
		return
	}
	defer os.RemoveAll(tmpDir)
	os.Chdir(tmpDir)

	subDirToSkip := "skip"

	fmt.Println("On Unix:")
	err = filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
		if err != nil {
			fmt.Printf("prevent panic by handling failure accessing a path %q: %v\n", path, err)
			return err
		}
		if info.IsDir() && info.Name() == subDirToSkip {
			fmt.Printf("skipping a dir without errors: %+v \n", info.Name())
			return filepath.SkipDir
		}
		fmt.Printf("visited file or dir: %q\n", path)
		return nil
	})
	if err != nil {
		fmt.Printf("error walking the path %q: %v\n", tmpDir, err)
		return
	}
}
```

















### 参考链接



https://www.ardanlabs.com/blog/2018/11/goroutine-leaks-the-forgotten-sender.html

https://www.ardanlabs.com/blog/2019/04/concurrency-trap-2-incomplete-work.html

https://www.ardanlabs.com/blog/2014/01/concurrency-goroutines-and-gomaxprocs.html

https://dave.cheney.net/practical-go/presentations/qcon-china.html#_concurrency

https://golang.org/ref/mem

https://blog.csdn.net/caoshangpa/article/details/78853919

https://blog.csdn.net/qcrao/article/details/92759907

https://cch123.github.io/ooo/

https://blog.golang.org/codelab-share

https://dave.cheney.net/2018/01/06/if-aligned-memory-writes-are-atomic-why-do-we-need-the-sync-atomic-package

http://blog.golang.org/race-detector



https://dave.cheney.net/2014/06/27/ice-cream-makers-and-data-races

https://www.ardanlabs.com/blog/2014/06/ice-cream-makers-and-data-races-part-ii.html

https://medium.com/a-journey-with-go/go-how-to-reduce-lock-contention-with-the-atomic-package-ba3b2664b549

https://medium.com/a-journey-with-go/go-discovery-of-the-trace-package-e5a821743c3c

https://medium.com/a-journey-with-go/go-mutex-and-starvation-3f4f4e75ad50

https://www.ardanlabs.com/blog/2017/10/the-behavior-of-channels.html

https://medium.com/a-journey-with-go/go-buffered-and-unbuffered-channels-29a107c00268

https://medium.com/a-journey-with-go/go-ordering-in-select-statements-fd0ff80fd8d6

https://www.ardanlabs.com/blog/2017/10/the-behavior-of-channels.html



https://www.ardanlabs.com/blog/2014/02/the-nature-of-channels-in-go.html

https://www.ardanlabs.com/blog/2013/10/my-channel-select-bug.html

https://blog.golang.org/io2013-talk-concurrency

https://blog.golang.org/waza-talk

https://blog.golang.org/io2012-videos

https://blog.golang.org/concurrency-timeouts

https://blog.golang.org/pipelines

https://www.ardanlabs.com/blog/2014/02/running-queries-concurrently-against.html

https://blogtitle.github.io/go-advanced-concurrency-patterns-part-3-channels/

https://www.ardanlabs.com/blog/2013/05/thread-pooling-in-go-programming.html

https://www.ardanlabs.com/blog/2013/09/pool-go-routines-to-process-task.html

https://blogtitle.github.io/categories/concurrency/



https://medium.com/a-journey-with-go/go-context-and-cancellation-by-propagation-7a808bbc889c

https://blog.golang.org/context

https://www.ardanlabs.com/blog/2019/09/context-package-semantics-in-go.html

https://golang.org/ref/spec#Channel_types

https://drive.google.com/file/d/1nPdvhB0PutEJzdCq5ms6UI58dp50fcAN/view

https://medium.com/a-journey-with-go/go-context-and-cancellation-by-propagation-7a808bbc889c

https://blog.golang.org/context

https://www.ardanlabs.com/blog/2019/09/context-package-semantics-in-go.html

https://golang.org/doc/effective_go.html#concurrency

https://zhuanlan.zhihu.com/p/34417106?hmsr=toutiao.io

https://talks.golang.org/2014/gotham-context.slide#1

https://medium.com/@cep21/how-to-correctly-use-context-context-in-go-1-7-8f2c0fafdf39