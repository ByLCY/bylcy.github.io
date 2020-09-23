# go语言初识

<!--more-->

## 安装 go 编译器

前往 golang [官网](https://golang.org/dl/)下载对应平台的安装包。如果因为特殊原因进不去那么可以访问[官方镜像](https://golang.google.cn/dl/)。

在安装完成后把安装目录下的bin目录添加PATH中。golang 会把用户文件夹下的 go 文件夹作为 GPATH来使用，如果想要更换可以设置环境变量 GOPATH 的值为你想要的目录，然后再目录下新建src、pkg和bin三个文件夹（在高版本下会自动创建和添加 bin 目录）。

## Hello World

按照惯例我们第一个程序就是Hello World。在记事本中将下列代码输入进去并且保存为 HelloWorld.go ，保存时注意将编码调整为UTF-8。

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World!!!")
}
```

由于 golang 是编译型语言所以要运行我们的代码就要先编译程序。 在按住 shift 的同时按下鼠标右键，选择“在此处打开 powershell 窗口”，然后输入 `go build HelloWorld.go`就会编译出 HelloWorld.exe 文件，在，命令行中输入 `.\HelloWorld.exe`。你将会看见如下输出：

```sh
Hello World!!!
```

{{< admonition type=tip title="技巧" open=true >}}
可以使用 `go run HelloWorld.go` 直接编译并运行程序。
{{< /admonition >}}

<br />

接下来我们通过这个程序来简单了解一下 go 的大致语法。

### 包的创建

go 程序都是由一个个包组成的。在我们的程序中，第一行`package main`就相当于创建了一个叫 **main** 的包。**main** 包会比其他包要特殊，因为 go 执行程序的入口是 main 函数，而想要 main 函数被执行的话，那么 main函数 必须位于 main 包下。

{{< admonition type=example title="示例" open=false >}}

当包名不为 main 时：

```go
package test

import "fmt"

func main() {
	fmt.Println("Hello World!!!")
}
```

运行效果：

{{< image src="/images/go%E5%88%9D%E8%AF%86/non-main-package.jpg" caption="non-main package" width="500px" >}}

当没有 main 函数时：

```go
package main

import "fmt"

func test() {
	fmt.Println("Hello World!!!")
}
```

运行效果：

{{< image src="/images/go%E5%88%9D%E8%AF%86/non-main-function.jpg" caption="non-main function" width="500px" >}}

以上例子证明：如果要写一个可执行程序的话程序的入口必须是在 main 包下的 main 函数。

{{< /admonition >}}

{{< admonition type=tip title="技巧" open=true >}}

使用 visual studio code 编辑器的体验会比记事本更好。

{{< /admonition>}}

### 包的导入

第三行的 `import “fmt”` 的作用是导入 fmt 包，这个包是 go 语言自带的包，无需另外下载。

导入包的语法还有 `import ()` 在括号中可以填入多个包。

```go
import (
	"package1"
    "package2"
    "package3"
)
```

{{< admonition type=info open=true >}}

**fmt** 包里是用于格式化 I/O 的函数，类似于 C 语言的 printf 和 scanf 。

{{< /admonition>}}

如果你学过其它编程语言，那么你会发现 go 语言的格式规范较为严格。在 go 中不允许导入无用包和定义未使用的变量，否则的话会在编译阶段报错。你可以用 `_` 来表示你只需要使用包里面的 init函数。导入的包也可以在导入是重命名或者是直接导入全部函数。

```go
import (
	_ "database/sql" // 只使用初始化函数
	f "flag" // 改名为 f，调用时用 f.func 的形式
	"fmt" // 正常导入，调用时用 fmt.func 的形式
	. "net/http" // 直接可以使用包内函数，不需要通过 package.func 的方式调用
)
```

### 函数

函数是代码复用的重要手段。函数的定义格式

```go
func 函数名(参数列表) 返回值 {
    函数体
}
```

在我们的 HelloWorld 程序中 的 main 函数的参数了返回值都没有填写，说明定义的这个函数没有参数和返回值。参数的传递一般有两种形式——值传递和引用传递。值传递就是将实参的值传递给形参，引用传递则是将地址传递给形参。值传递不会改变原变量，而引用传递会改变。在 golang 中一般默认为值传递。

函数的闭包和导出。函数名的第一个字母为大写说明此函数导出，导出的函数可以方便的被其它程序引用。与此相反函数名第一个字母为小写，则说明此函数为闭包。在其它程序导入此包时无法直接引用此函数。

## 结语

本篇文章通过一个HelloWorld程序简单且初略的说明了 go 程序的大概结构。还有许多地方是没有讲清楚，甚至是没有讲的。这个教程将会是一个[系列](http://bylcy.github.io/categories/go-tutorial/)待我在后面慢慢补足。

本系列虽然名为教程，但实际上是我个人的知识回顾整理。且这是本人第一次写blog，还有什么不足之处还望大伙门能在评论区提出来，望大家共同进步。
