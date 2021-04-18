---
title: Go语言基础
toc: true
thumbnail: /gallery/thumbnails/golang.png
widgets:
  - type: profile
    position: left
    author: 博雅
    author_title: zju小硕一枚
    location: 'Hangzhou,China'
    avatar: /images/Golden_Medal_-1_Icon.svg
    gravatar: null
    avatar_rounded: false
    follow_link: 'https://github.com/MasterXuBoya'
    social_links:
      Github:
        icon: fab fa-github
        url: 'https://github.com/MasterXuBoya'
      E-Mail:
        icon: fa fa-envelope
        url: xuboya@zju.edu.cn
      Facebook:
        icon: fab fa-facebook
        url: 'https://facebook.com'
      Twitter:
        icon: fab fa-twitter
        url: 'https://twitter.com'
      RSS:
        icon: fas fa-rss
        url: /
  - type: category
    position: left
  - type: toc
    position: left
date: 2020-04-01 17:39:06
tags:
	- 编程语言
	- Golang
	- Gin
	- gRPC
categories:
	- 编程语言
	- Golang
typora-root-url: Go语言基础
---

---

## Go语言基础

### 变量&常量

不同于C/C++，Go的变量类型放在变量名之后,以下是常见变量类型的对比

<!--more-->

| Go                                    | C                                          |
| ------------------------------------- | ------------------------------------------ |
| var a int                             | int a;                                     |
| var a *int                            | int* a;                                    |
| var a []int                           | int a[];                                   |
| var a [][] [] [] int                  | int a[] [];                                |
| var a []map[int] string               | vector<map<int,string> > a;                |
| var t map[string] []map[string]string | map<string,vector<map<int ,string > > > a; |

> Go语言中，类型声明这样设计的哲学据说是为了克服C语言的复杂性。当多个类型嵌套或者使用函数指针类型时，C语言类型变得很复杂，不易懂，Go简化了这一过程

`:=`的含义也不同于Pascal，在Go语言中，该符号表示声明并赋值，免去var声明过程

```go
var a int
a=100
b:=100
```

常量最常见的用法是使用iota来模拟枚举类型

```go
const (
		n1 = iota //0
		n2        //1
		n3        //2
		n4        //3
	)
```



### 基本数据类型 

+ 整型：int|int8、int16、int32、int64|uint8、uint16、uint32、uint64

  > int类型根据计算机的实际位数在不同电脑有不同的表示

+ 浮点型：float32、float64

+ 布尔型：true、false

+ 字符串：string|byte|rune

  > byte=uint8，适用于英文string；rune=int32，用于包含其他语言的string
  >
  > ```go
  > s := "hello world"
  > for i := 0; i < len(s); i++ { //byte
  >     fmt.Printf("%v(%c) ", s[i], s[i])
  > }
  > s := "hello 中国"
  > for _, r := range s { //rune
  >     fmt.Printf("%v(%c) ", r, r)
  > }
  > ```

> Go语言没有隐式类型转换，只有强制类型转换
>
> ```go
> var a int=100
> var b float64
> b=float64(a)   //int-->double 依然需要强制类型转换
> ```
>
> 

### 引用类型

0. 数组、切片、map

> Go语言中函数的参数有两种传递方式，按**值传递**和按**引用传递**.
>
>  Go默认使用按值传递（函数形参为基本数据类型和数组）来传递参数，也就是传递参数的副本。在函数中对副本的值进行更改操作时，不会影响到原来的变量。
>
> ```go
> func add(a int,b int)int{}//副本
> func add(a []int,b int){}//副本
> ```
>
> 数组经常使用的是指针传递或者传入slice
>
> ```go
> func add(a *[5]int, n int) int { //指向数组的指针
> 	s := 0
> 	for i := 0; i < n; i++ {
> 		s += (*a)[i]
> 	}
> 	(*a)[0] = 10
> 	return s
> }
> func main(){
>     var a [5]int
>     sum:=add(&a,5)//此时a[0]已经发生改变
> }
> ```
>
> 引用类型（slice、map、interface、channel）都默认使用引用传递,可以返回参数的改变量
>
> ```go
> func sum(arr []int) int {}
> func main() {
>     var arr = [5]int{1, 2, 3, 4, 5}
>     fmt.Println(sum(arr[:]))
> }
> ```

1. 数组固定长度

```go
for i:=0;i<n;i++{//访问实际的元素
    fmt.Println(a[i])
}
for k,v:=range a{//该方法访问数组的所有元素——Capibility个
    fmt.Println(k,v)
}
```

2. slice切片类似于C++中的Vector（Python的list），使用内置的`len()`函数求长度，使用内置的`cap()`函数求切片的容量

+ 声明slice

```go
var a []int
var s []string
```

> 不同于数组，此处无需定义数组的容量，slice类型

+ 初始化

```go
a:=[]int{1,2,3,4,5}
b:=a[1:3]//通过数组获得切片

s := make([]int, 2, 10)//通过make获得切片
```

+ 切片操作

```go
a[low,high]//a[low≤index<high]
```

+ 引用传递——同第0节内容，切片可以带回函数变化量

3. Map对应于C++里面的Map、Python里的Dict，底层是Hash散列实现的数据结构，常见的基本操作有以下几种

+ 定义Map

```go
var a map[KeyType]ValueType//声明
a=make(map[string]int)

s:=make(map[KeyType]ValueType, [cap])//类似指针的new、malloc操作，分配空间
```

+ 遍历Map

```go
for k, v := range scoreMap {}//遍历<key,value>
for k := range scoreMap {}//遍历key
```

+ 查询某个Key是否存在

```go
v, ok := scoreMap["张三"]
if ok {//存在
    
} else {//不存在
    
}
```

+ 添加、修改某个<Key,value>

```go
m[key]=value
```

+ 删除某个key

```go
delete(map, key)
```

### 函数与闭包

0. 函数类型（底层应该是函数指针）：

```go
type calculation func(int, int) int
func calc(x, y int, op func(int, int) int) int {//函数类型作为形参
```

1. 闭包：闭包指的是一个函数和与其相关的引用环境组合而成的实体。在函数中不可以声明有名函数，但是可以申明匿名函数，此匿名函数可以共享该函数的外部参数

```go
func adder() func(int) int {
	var x int
	return func(y int) int {
		x += y
		return x
	}
}
```

> `adder`函数返回的是一个函数类型（C/C++中的函数指针）

为什么要使用闭包？闭包是为了解决什么问题而引入的？

​		别人已经写好的函数库中的**回调函数**只能传入`func()`的函数类型，但是我们写的函数类型是`func(int,int)int`，此时如何把这个函数通过参数传入进去

```go
func callback(f func()){//别人库中的回调函数
    f()
}
--------------------------------
func myfunc(x,y int){//个人实现的功能性函数
    fmt.Println(x+y)
}
//该函数负责转换，将函数类型转化成callback函数可以接受的类型
func convert(f func(int,int),x int,y int)func(){//f,x,y变量可以在匿名函数中访问
    return func(){
        f(x,y)
    }
}

func main(){
    x:=1
    y:=2
    newf:=convert(myfunc,x,y)
    callback(newf)
}
```

>  Tips: `convert`函数：`func(int,int)int`-->`func()`
>
> 执行`callback(newf)`其实就是在执行`f(x,y)`，x和y变量已经被提前注入了

2. defer

Go语言中的`defer`语句会将其后面跟随的语句进行延迟处理。在`defer`归属的函数即将返回时，将延迟处理的语句按`defer`定义的逆序进行执行，也就是说，先被`defer`的语句最后被执行，最后被`defer`的语句，最先被执行。

由于`defer`语句延迟调用的特性，所以`defer`语句能非常方便的处理资源释放问题。比如：资源清理、文件关闭、解锁及记录时间等。

![defer](Go语言基础/defer.png)

### 反射

> 给出一个变量，利用反射机制可以得到该该变量的元数据信息，最简单的元数据就是数据类型

reflect包提供了`reflect.TypeOf`和`reflect.ValueOf`两个函数来获取任意对象的Value和Type.

```go
type student struct {
	Name  string `json:"name"`
	Score int    `json:"score"`
}

func main() {
	stu1 := student{
		Name:  "小王子",
		Score: 90,
	}

	t := reflect.TypeOf(stu1)
	fmt.Println(t.Name(), t.Kind()) // student struct
	// 通过for循环遍历结构体的所有字段信息
	for i := 0; i < t.NumField(); i++ {
		field := t.Field(i)
		fmt.Printf("name:%s index:%d type:%v json tag:%v\n", field.Name, field.Index, field.Type, field.Tag.Get("json"))
	}

	// 通过字段名获取指定结构体字段信息
	if scoreField, ok := t.FieldByName("Score"); ok {
		fmt.Printf("name:%s index:%d type:%v json tag:%v\n", scoreField.Name, scoreField.Index, scoreField.Type, scoreField.Tag.Get("json"))
	}
}
```

反射机制的典型应用：

+ Json格式与struct的相互转换 

```go
//Student 学生
type Student struct {
	ID     int
	Gender string
	Name   string
}

//Class 班级
type Class struct {
	Title    string
	Students []*Student
}
str := `{"Title":"101","Students":[{"ID":0,"Gender":"男","Name":"stu00"},{"ID":1,"Gender":"男","Name":"stu01"},{"ID":2,"Gender":"男","Name":"stu02"},{"ID":3,"Gender":"男","Name":"stu03"},{"ID":4,"Gender":"男","Name":"stu04"},{"ID":5,"Gender":"男","Name":"stu05"},{"ID":6,"Gender":"男","Name":"stu06"},{"ID":7,"Gender":"男","Name":"stu07"},{"ID":8,"Gender":"男","Name":"stu08"},{"ID":9,"Gender":"男","Name":"stu09"}]}`
c1 := &Class{}
err = json.Unmarshal([]byte(str), c1)
if err != nil {
    fmt.Println("json unmarshal failed!")
    return
}
fmt.Printf("%#v\n", c1)
```

编写`Unmarshal`函数的的库程序员只能获取`c1`变量，利用反射机制推断出其对应的struct结构

+ [Ini配置文件读入到struct](#Ini配置文件解析（反射机制）)

### 并发

#### 协程

​		协程(goroutine)是一个比线程还要轻量级的用户态线程，我们可以根据需要创建成千上万个`goroutine`并发工作。

其他语言的线程都是创建在内核态线程？？？

​		OS线程（操作系统线程）一般都有固定的栈内存（通常为**2MB**）,一个`goroutine`的栈在其生命周期开始时只有很小的栈（典型情况下**2KB**），`goroutine`的栈不是固定的，他可以按需增大和缩小，`goroutine`的栈大小限制可以达到1GB，虽然极少会用到这个大。所以在Go语言中一次创建十万左右的`goroutine`也是可以的。

​		`GPM`是Go语言运行时（runtime）层面的实现，是go语言自己实现的一套调度系统。单从线程调度讲，Go语言相比起其他语言的优势在于OS线程是由OS内核来调度的，`goroutine`则是由Go运行时（runtime）自己的调度器调度的，这个调度器使用一个称为m:n调度的技术（复用/调度m个goroutine到n个OS线程）。

+ 创建goroutine

```go
var wg sync.WaitGroup

func func1(out <-chan int,in chan<- int){//只能从chan取数,不可写入
    defer wg.Done()//协程数-1
    for {
			i, ok := <-ch1 // 通道关闭后再取值ok=false
			if !ok {
				break
			}
			ch2 <- i * i
    }
    close(in)
    close(out)
}
func main(){
    var in chan int
    var out chan int
    
    in=make(chan int,100)
    out=make(chan int,100)
    
    for i=0;i<100;i++{
        out<-i//主协程写入数据
    }
    wg.Add(1)//协程数+1
    go func1(out,in)//启动协程
    
    wg.Wait()//等待所有协程结束
}
```

`go func(parm)`参数直接通过形参传入协程

#### 协程间通信

> 协程间数据通信存在两种方式，channel和共享全局变量

0. channel

+ 三种基本操作：发送、接收、关闭

```go
ch<-x//发送
x:=<-ch//接收
close(ch)//关闭
```

+ 两种模式：无缓冲channel和有缓冲channel

```go
ch := make(chan int)
ch := make(chan int, 10) // 创建一个容量为1的有缓冲区通道
```

> 无缓冲通道上的发送操作会阻塞，直到另一个`goroutine`在该通道上执行接收操作，这时值才能发送成功，两个`goroutine`将继续执行。相反，如果接收操作先执行，接收方的goroutine将阻塞，直到另一个`goroutine`在该通道上发送一个值。
>
> 有缓冲的channel，当缓冲区满即阻塞，等待别的协程把数据取出

![channel](Go语言基础/channel01.png)

**最常用的方式**

​		g1协程写入数据到c1 channel中，可以开启多个goroutine通过读取c1的数据，并行处理c1的数据，处理后的数据存入c2 channel

![channel](/../../gallery/images/channel典型应用图.jpg)

1. 共享全局变量

+ 互斥锁

> 互斥锁本质上是一种特殊的信号量，资源数量为1

```go
var lock sync.Mutex


lock.Lock()
XXXXXXXXXXX
lock.Unlock()
```

+  读写锁

> g1协程读取的时候，别的协程依然可以读取；g1协程写入的时候，拥有对资源的绝对使用权，别的协程不能读取和写入

```go
rwlock sync.RWMutex

rwlock.RLock()
读取共享变量
rwlock.RUnlock()

rwlock.Lock()
写入共享变量
rwlock.Unlock()
```

+ sync.Once

> 用于在多个goroutine中只能执行一次的函数，内部通过mutex互斥锁实现
>
> 典型应用场景：例如只加载一次配置文件、**只关闭一次通道等**

```go
var loadIconsOnce sync.Once

func f(){//自定义需要执行的函数
    
}

loadIconsOnce.Do(f)
```

+ sync.Map

> Go语言中内置的map不是并发安全的,因此需要使用sync.Map
>
> `sync.Map`内置了诸如`Store`、`Load`、`LoadOrStore`、`Delete`、`Range`等操作方法,使用方法类似，仅仅是换了一个函数名

```go
var m = sync.Map{}

func main() {
	wg := sync.WaitGroup{}
	for i := 0; i < 20; i++ {
		wg.Add(1)
		go func(n int) {
			key := strconv.Itoa(n)
			m.Store(key, n)
			value, _ := m.Load(key)
			fmt.Printf("k=:%v,v:=%v\n", key, value)
			wg.Done()
		}(i)
	}
	wg.Wait()
}

```

+ 原子操作

​		`x++`命令内部实现需要三个步骤，因此在某一个线程执行该命令时，可能被其它线程打断，造成无可预料的错误，这就是说该命令不是**原子**的。`sync/atomic`包提供了很多原子操作的方法，里面的函数在执行时都会在一个机器指令周期完成， 不会被别的协程打断保证命令的原子性。类似`TestAndSet`，个人估计原子操作函数内部可能通过关闭中断实现

```go
import ""sync/atomic""

atomic.AddInt64(&x, 1)
```



### 网络编程（net库）

![socket抽象层](/../../gallery/images/socket.png)

#### Socket编程

> golang的socket编程比C++简化了很多

0. 实现TCP协议

+ server

```go
// tcp/server/main.go

// TCP server端

// 处理函数
func process(conn net.Conn) {
	defer conn.Close() // 关闭连接
	for {
		reader := bufio.NewReader(conn)
		var buf [128]byte
		n, err := reader.Read(buf[:]) // 读取数据
		if err != nil {
			fmt.Println("read from client failed, err:", err)
			break
		}
		recvStr := string(buf[:n])
		fmt.Println("收到client端发来的数据：", recvStr)
		conn.Write([]byte(recvStr)) // 发送数据
	}
}

func main() {
	listen, err := net.Listen("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	for {
		conn, err := listen.Accept() // 建立连接
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn) // 启动一个goroutine处理连接
	}
}
```

流程：Listen-->Accept(三次握手)-->Read-->Close

​		`Accept`获得的`conn`为`client`句柄，包含client的信息。server读取数据的时候把conn网口当做是file类型，`reader := bufio.NewReader(conn)`

+ client

```go
// tcp/client/main.go

// 客户端
func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("err :", err)
		return
	}
	defer conn.Close() // 关闭连接
	inputReader := bufio.NewReader(os.Stdin)
	for {
		input, _ := inputReader.ReadString('\n') // 读取用户输入
		inputInfo := strings.Trim(input, "\r\n")
		if strings.ToUpper(inputInfo) == "Q" { // 如果输入q就退出
			return
		}
		_, err = conn.Write([]byte(inputInfo)) // 发送数据
		if err != nil {
			return
		}
		buf := [512]byte{}
		n, err := conn.Read(buf[:])
		if err != nil {
			fmt.Println("recv failed, err:", err)
			return
		}
		fmt.Println(string(buf[:n]))
	}
}
```

Dial-->Write-->Close

1. 实现UDP协议

+ Server

```go
// UDP/server/main.go

// UDP server端
func main() {
	listen, err := net.ListenUDP("udp", &net.UDPAddr{
		IP:   net.IPv4(0, 0, 0, 0),
		Port: 30000,
	})
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	defer listen.Close()
	for {
		var data [1024]byte
        n, addr, err := listen.ReadFromUDP(data[:]) // 接收数据,n:数据长度，addr：客户端句柄
		if err != nil {
			fmt.Println("read udp failed, err:", err)
			continue
		}
		fmt.Printf("data:%v addr:%v count:%v\n", string(data[:n]), addr, n)
		_, err = listen.WriteToUDP(data[:n], addr) // 发送数据
		if err != nil {
			fmt.Println("write to udp failed, err:", err)
			continue
		}
	}
}
```

流程：ListenUDP-->ReadFromUDP-->Close

> 没有Accept三次握手连接的过程

+ Client

```go
// UDP 客户端
func main() {
	socket, err := net.DialUDP("udp", nil, &net.UDPAddr{
		IP:   net.IPv4(0, 0, 0, 0),
		Port: 30000,
	})
	if err != nil {
		fmt.Println("连接服务端失败，err:", err)
		return
	}
	defer socket.Close()
	sendData := []byte("Hello server")
	_, err = socket.Write(sendData) // 发送数据
	if err != nil {
		fmt.Println("发送数据失败，err:", err)
		return
	}
    
	data := make([]byte, 4096)
	n, remoteAddr, err := socket.ReadFromUDP(data) // 接收数据
	if err != nil {
		fmt.Println("接收数据失败，err:", err)
		return
	}
	fmt.Printf("recv:%v addr:%v count:%v\n", string(data[:n]), remoteAddr, n)
}

```

流程：DialUDP-->Write-->Close

此处有一个问题：client发送数据后立马就接收数据，`	n, remoteAddr, err := socket.ReadFromUDP(data)`

#### Http服务器

​		`net/http`提供了http协议的相关API，该库应该是建立在socket库`net`之上，进一步封装，实现http协议。利用socket编程也可以在定义消息实现http协议的，但是该方案需要自己封装帧，稳定性等方面也不如go提供的原生态`net/http`。

​		实际开发中，大部分时间应该在用框架，如何gin进行业务系统的开发，这种原生态的自己编写socket或者http协议的代码仅用作学习使用

+ get请求

```go
func main() {
	resp, err := http.Get("https://www.liwenzhou.com/")
	if err != nil {
		fmt.Println("get failed, err:", err)
		return
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("read from resp.Body failed,err:", err)
		return
	}
	fmt.Print(string(body))
}
```

resp是get请求返回的内容，利用ioutil.ReadAll读取所有对象

+ server

```go
func sayHello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Hello 沙河！")
}

func main() {
	http.HandleFunc("/", sayHello)
	err := http.ListenAndServe(":9090", nil)
	if err != nil {
		fmt.Printf("http server failed, err:%v\n", err)
		return
	}
}
```

​		server监听某个路径（如/login），传入某个函数类型，指定处理函数handle。函数传入固定的参数类型`w http.ResponseWriter, r *http.Request`,gin等go框架应该会对该功能进一步细化。

## 常用标准库

### 文件相关 OS包

+ 打开文件

```go
file, err := os.Open(fileName)
```

返回文件对象句柄

+ 关闭文件

```go
file.Close()
```

+ 重命名文件

```go
os.Rename(oldName,newName)
```

+ 常见的Linux文件命令，都可以在os包中找到，如mkdir()、Remove()、Chown()

### 文件IO操作：fmt & bufio & io/ioutil

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"io/ioutil"
	"os"
)

var fileName string = "./1.txt"   // ./表示当前目录

//ReadByFile :利用*file类型读取，读取固定长度的内容
func ReadByFile(fileName string) {
	file, err := os.Open(fileName)
	if err != nil {
		fmt.Println("Open file failed,the error message is:", err)
		return
	}
	defer file.Close()

	b := make([]byte, 10) //10个10个读
	for {
		len, err := file.Read(b)
		if err == io.EOF {
			fmt.Print(string(b[:len]))
			return
		}
		if err != nil {
			fmt.Println("Read Failed ,the error message is:", err)
			return
		}
		fmt.Print(string(b[:len]))
	}
}

//ReadByBufio 通过bufio包读，一行一行读取。
func ReadByBufio(fileName string) {
	file, err := os.Open(fileName)
	if err != nil {
		fmt.Println("Open file failed,the error message is:", err)
		return
	}
	defer file.Close()
	reader := bufio.NewReader(file)
	for {
		str, err := reader.ReadString('\n')//不适用readLine函数
		if err == io.EOF {
			if len(str) != 0 {
				fmt.Print(str)
			}
			break
		}
		if err != nil {
			fmt.Println("reader readline failed,the error message is:", err)
			return
		}
		fmt.Print(str)
	}

}

//ReadByIoutil 通过Ioutil读取文件所有数据
func ReadByIoutil(fileName string) {
	data, err := ioutil.ReadFile(fileName)//该包不需要os.file操作
	if err != nil {
		fmt.Println("ioutil read failed,the error message is:", err)
		return
	}
	fmt.Print(string(data))
}

func main() {
	ReadByFile(fileName)
	fmt.Println("\n")
	ReadByBufio(fileName)
	fmt.Println("\n")
	ReadByIoutil(fileName)
}

```

### 字符串操作：strings & strconv



### 数学计算

## Go操作Web组件

### 后端框架Gin



### 数据库

#### MySQL



#### Redis

0. 为何要有Redis

​        Redis是一个<key,value>形式的内存数据库，基本的功能是当做一个缓存使用。如果要实现一个缓冲，朴素的思想，在程序中简单使用一个Map或者其他数据存储数据即可，为什么还要使用Redis呢？

原因：Redis除了基本的数据存储外，还提供事务机制、数据备份和恢复、安全性保证、性能测试等多个功能，保证程序的高可用性。可以把Redis当做一个高可用的整体系统，很多高并发、安全、数据备份等功能该系统已经帮我们完成了，不需要自己再重新实现了。从这点来考虑，就类似于数据库系统，简单的文件系统也可以实现持久化存储功能，为什么还要使用数据库呢？除了数据库SQL简洁方便外，还因为数据库提供了安全性、高并发、数据恢复、原子性等多个原因。后端主要就是解决这些性能问题，如果只是实现基本功能后端程序完全可以很简单，就是因为有个各种性能要求才会变得更加复杂

​		Redis系统采用c/s模式，包括redis-server和redis-client，同时对外暴露接口，供CMD命令行或者各种语言API调用。通过ping的方式可以判断是否建立client和server之间的通道连接

​		Redis目前在互联网各个领域已经取代了古老的Memcached

1. Redis中value的数据类型和使用方法

Redis是键值对数据库，value的数据类型常见的有以下5种：

string、Hash、List、Set、Sorted Set

+ string

bash命令

```bash
set key value
get key
del key
Exist key
```

go命令

```go
func redisExample() {
	err := rdb.Set("score", 100, 0).Err()
	if err != nil {
		fmt.Printf("set score failed, err:%v\n", err)
		return
	}

	val, err := rdb.Get("score").Result()
	if err != nil {
		fmt.Printf("get score failed, err:%v\n", err)
		return
	}
	fmt.Println("score", val)

	val2, err := rdb.Get("name").Result()
	if err == redis.Nil {
		fmt.Println("name does not exist")
	} else if err != nil {
		fmt.Printf("get name failed, err:%v\n", err)
		return
	} else {
		fmt.Println("name", val2)
	}
}
```

go语言API的函数名称基本与命令行的命令名称类似

+ hash

```bash
HMSET key field1 value1 [field2 value2 ] 
HSET key field value 
HGET key field 
HGETALL key 
```

+ List

```bash
LPUSH key value1 [value2] //左侧插入
RPUSH key value1 [value2] //右侧插入
LPUSHX key value  //左侧插入一个
RPOP key //右侧pop
```

+ Set

```bash
SADD key member1 [member2] //添加元素
SMEMBERS key //返回所有成员
```

+ Sorted Set(zset)

```bash
ZADD key score1 member1 [score2 member2] //添加多个成员和分数
ZINCRBY key increment member //key的member成员score+increment
查询过程有多个多种函数
```

> 有序集合的成员是唯一的,但分数(score)却可以重复。每个元素都会关联一个double类型的分数,redis正是通过分数来为集合中的成员进行从小到大的排序。

2. Redis典型应用场景

- 缓存系统，减轻主数据库（MySQL）的压力。
- 计数场景，比如微博、抖音中的关注数和粉丝数。
- 热门排行榜，需要**排序**的场景特别适合使用ZSET。
- 利用LIST可以实现队列的功能。

| String(字符串)       | 二进制安全                                             | 可以包含任何数据,比如jpg图片或者序列化的对象,一个键最大能存储512M | ---                                                          |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Hash(字典)           | 键值对集合,即编程语言中的Map类型                       | 适合存储对象,并且可以像数据库中update一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去) | 存储、读取、修改用户属性                                     |
| List(列表)           | 链表(双向链表)                                         | 增删快,提供了操作某一段元素的API                             | 1,最新消息排行等功能(比如朋友圈的时间线) 2,消息队列          |
| Set(集合)            | 哈希表实现,元素不重复                                  | 1、添加、删除,查找的复杂度都是O(1) 2、为集合提供了求交集、并集、差集等操作 | 1、共同好友 2、利用唯一性,统计访问网站的所有独立ip 3、好友推荐时,根据tag求交集,大于某个阈值就可以推荐 |
| Sorted Set(有序集合) | 将Set中的元素增加一个权重参数score,元素按score有序排列 | 数据插入集合时,已经进行天然排序                              | 1、排行榜 2、带权重的消息队列                                |

3. Golang操作Redis的方法（MySQL也是类似的方法）

+ 在PC机上安装Redis，通过命令行工具测试是否安装成功
+ 利用CMD工具打开redis-client，可以直接对Redis进行操作，类似MySQL数据库
+ 在Golang中安装第三方Redis库，使用`go get`命令
+ 在Golang程序中编写代码，**连接**Redis并**控制**



4. 待研究内容：

+ Redis内部原理——解决的主要问题，研究大的方向类似数据库
+ 分布式Redis
+ Redis持久化存储
+ Redis实践
+ [Redis Reference](https://blog.csdn.net/bernkafly/article/details/89553711)

#### MongoDB



### Web中间件



### rRPC



## Go实际项目

### Ini配置文件解析（反射机制）

+ 为什么使用反射机制

​		之所以该功能需要利用反射的原因在于并不知道传入的结构体变量x的**具体类型和字段名**是什么，因此只能利用反射。**该解析ini解析函数相当于底层库，是一个通用函数，供上层程序员调用。**如果该ini解析函数库与调用该函数的程序员属于同一个人，那么就可以知道struc的具体结构了，就可以有更加简单的写法，也不用使用反射机制。

+ 思路

​		具体来说：从ini文件中每读入一行进行判断，判断是空行、标题、字段、非法行。如果是有效字段，就进行分割成key和value，利用反射机制在x的struct中遍历字段，看是否有某个字段的tag和key相同，然后将该字段的值赋值成value。

### 日志库

+ 需求分析

1. 定义6种日志级别：Debug、Trace、Info、Warning、Error、Fatal

> 为了实现功能3，定义日志Level类型（枚举类型），
>
> ```go
> type LogLevel uint32
> const {
>     DEBUG LogLevel iota
>     TRACE
>     INFO
>     WARNING
>     ERROR
> 	FATAL
> }
> //log调用方法
> func testLog(){
> 	log.Debug("This is a debug log")
> 	log.Warning("This is a warning log")
>     log.Error("This is a error log")
> }
> ```

2. 日志内容包括：时间、日志级别、文件名、调用函数名、行号、日志信息

> 时间通过time库实现
>
> runtime.caller(skip int) 文件名、调用函数名、行号

3. 定制阈值level，大于level级别的日志方才输出

```go
func (f *ConsoleLogger)Debug(msg string){
    Debug>=f.level时才输出日志
}
```

4. 定制开关，支持输出到console和文件

Logger包

​	Logger 接口

​		Debug()、Trace()、Info()……

​	ConsoleLogger类

​		Debug()、Trace()、Info()……NewConsoleLogger()、check()

​	FileLogger类

​		Debug()、Trace()、Info()……NewFileLogger()、check()

Main包

​	

ConsoleLogger类

```go
type ConsoleLogger struct{
    level LogLevel //阈值level
}
```

FileLogger类

```go
type ConsoleLogger struct{
    level LogLevel 		//阈值level
    filePath string  	//日志路径
    fileName string		//日志名称
    fileobj *os.file	//日志文件对象
    fileobjerr *os.file //错误级别以上日志对象
    maxSize int64		//最大文件大小
}
```

5. 日志切割，根据文件大小或者时间进行切割

每次写入日志前判断一下当前日志文件大小`fileobj.stat().size()`,小于阈值直接写入，大于该阈值则重新命名日志文件xx.log.bak20200320，并重新打开日志文件写入。

### 待研究内容

[Redis](#redis)



## Reference

[Go API中文文档](https://studygolang.com/pkgdoc)

[Go与C语言对比](https://hyperpolyglot.org/c#files)

[李文周Go语言技术博客](https://www.liwenzhou.com/posts/Go/go_menu/)





