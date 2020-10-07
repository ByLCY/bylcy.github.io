# Go 关键字

<!--more-->

## Go 关键字

|           |        |         |             |          |
| --------- | ------ | ------- | ----------- | -------- |
| break     | case   | chan    | const       | continue |
| default   | defer  | else    | fallthrough | for      |
| func      | go     | goto    | if          | import   |
| interface | map    | package | range       | return   |
| select    | struct | switch  | type        | var      |

## 保留字分类

### 基本结构

#### 包管理

1. package
2. import

{{< admonition type="info" open=true >}}
**package**关键字：

1. 在每个源文件的开头都有一个 `package` 声明，用于表示代码所属的包。
2. Go 的可执行程序必须有一个 `main` 包和一个 `main()` 函数。这个包和函数是整个可执行程序的入口。
3. 同一个路径下只能存在一个包，一个包可以拆分成多个源文件。

{{< admonition type="tip" open=true title="建议">}}

包名最好和当前文件夹名相同。

{{</admonition >}}

**import**关键字：

1. `import` 用于导入依赖的 `package`。
2. 在导入多个包时将按顺序执行初始化函数 `init()`。
3. 如果导入的A包含有B包，会先执行B包的初始化函数。
4. 导入的包可以取别名。如果别名是 `.` 则表示省略包名。
5. Go 不允许导入无用的包。如果只是为了执行 `init()`  函数可以将别名取为 `_`

```Go
import _ "packageName"  // 只执行 init() 函数

import (
    "packageName"  // 多包导入的推荐用法
    "packageName"
)
```



{{< /admonition >}}

#### 变量与常量

1. var
2. const

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 基本组件

#### 函数

1. func
2. return

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 自定义类型

1. interface
2. struct
3. type

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 引用类型

1. map
2. range

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 流程控制

go是一个原生就支持并发的编程语言，这个分类中就有特别体现go语言特点的并发控制。

#### 并发控制

1. go
2. select
3. chan

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 单任务流程控制

1. for
2. goto
3. if
4. else
5. break
6. switch
7. case
8. default
9. fallthrough
10. continue

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 延时控制

1. defer

{{< style "color:red" >}}~~[相关介绍](/)~~ **施工中**{{< /style >}}

### 结语

后续 Go 教程的的更新会按照上文的介绍来。较为简短的我就会用提示的方法直接在本篇文章中更新，较长的就另写一篇文章。
