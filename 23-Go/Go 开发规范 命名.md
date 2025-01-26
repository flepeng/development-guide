
# 0、整体

Go 中的惯例是使用 MixedCaps 或 mixedCaps 而不是下划线来书写多词名称。但是有三个例外：

*   仅由生成的代码导入的包名称可能包含下划线。
*   文件内的测试、基准和示例函数名称 `*_test.go` 可能包含下划线。
*   与操作系统或 cgo 交互的低级库可能会重用标识符。如 `_windows.go`


# 1、包名

*   全部小写，没有大写或下划线。
*   简短而简洁。
*   不用复数。例如 `net/url`，而不是`net/urls`。
*   不要用 `common、util、shared、lib`,这些不好、信息量不足的名称。
*   大多数使用命名导入的情况下，不需要重命名。
*   其不需要在所有源代码中是唯一的。

如 `tabwriter`，不是 `tabWriter`，`TabWriter` 或 `tab_writer`


## 1.1、最后一个词不是包名

如果程序包名称与导入路径的最后一个元素不匹配，则必须使用导入别名。

```go
import (
  "net/http"

  client "example.com/client-go"  
  trace "example.com/trace/v2" // 最后一个元素是v2
)
```


## 1.2、有冲突时

在所有其他情况下，除非导入之间有直接冲突，否则应避免导入别名。

```go
// 反面示例
import (
  "fmt"
  "os"

  nettrace "golang.net/x/trace" // 没有冲突，不使用别名
)

// 有冲突时，再起别名
import (
  "fmt"
  "os"
  "runtime/trace"

  nettrace "golang.net/x/trace"
)
```


# 2、文件名

尽量采取有意义的文件名，简短，有意义，应该为**小写**单词。

在分割单词上有两种意见：

*   使用**下划线**分隔各个单词：`approve_service.go`
*   全部使用小写，不做特殊处理，官方代码基本上都是这么做的：`approveservice.go`，特殊情况除外`*_test.go`


# 3、函数名

我们遵循 Go 社区关于使用 [MixedCaps 作为函数名](https://golang.org/doc/effective_go.html#mixed-caps) 的约定,使用驼峰命名法，不要使用下划线。举例：`MixedCaps` 或者`mixedCaps`

> 有一个例外，为了对相关的测试用例进行分组，函数名可能包含下划线，如：`TestMyFunction_WhatIsBeingTested`


# 4、结构体名

*   采用驼峰命名法，首字母根据访问控制大写或者小写
*   结构体名应该是名词或名词短语，如 Customer、WikiPage、Account、AddressParser，它不应是动词
*   struct 申明和初始化格式采用多行，例如下面：
        
    ```go
    type MainConfig struct {
        Port    string `json:"port"`
        Address string `json:"address"`
    }
    config := MainConfig{"1234", "123.221.134"}
    ```
    

# 5、接口名

*   命名规则基本和上面的结构体类型
    
*   单个方法的接口名以 “er” 作为后缀，例如 Reader, Writer 。
    
    ```go
    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    ```

*   两个函数的接口名综合两个函数名。

*   三个以上函数的接口名，类似于结构体名

    ```
    type Car interface {
        Start([]byte)
        Stop() error
        Recover()
    }
    ```


# 6、变量

*   和结构体类似，变量名称一般遵循驼峰法，首字母根据访问控制原则大写或者小写（导出的常量以大写字母开头，而未导出的常量以小写字母开头），

*   遇到特有名词时，需要遵循以下规则：
    *   如果变量为私有，且特有名词为首个单词，则使用小写，如 appService
    *   其他情况都应该使用该名词原有的写法，如 APIClient、repoID、UserID；
    *   错误示例：UrlArray，应该写成 urlArray 或者 URLArray；

    | English Usage | 范围  | 正确的      | 错误的                                    |
    |---------------|-----|----------|----------------------------------------|
    | XML API       | 已导出 | `XMLAPI` | `XmlApi`, `XMLApi`, `XmlAPI`, `XMLapi` |
    | XML API       | 未导出 | `xmlAPI` | `xmlapi`, `xmlApi`                     |
    | iOS           | 已导出 | `IOS`    | `Ios`, `IoS`                           |
    | iOS           | 未导出 | `iOS`    | `ios`                                  |
    | gRPC          | 已导出 | `GRPC`   | `Grpc`                                 |
    | gRPC          | 未导出 | `gRPC`   | `grpc`                                 |
    | DDoS          | 已导出 | `DDoS`   | `DDOS`, `Ddos`                         |
    | DDoS          | 未导出 | `ddos`   | `dDoS`, `dDOS`                         |
    | ID            | 已导出 | `ID`     | `Id`                                   |
    | ID            | 未导出 | `id`     | `iD`                                   |
    | DB            | 已导出 | `DB`     | `Db`                                   |
    | DB            | 未导出 | `db`     | `dB`                                   |
    | Txn           | 已导出 | `Txn`    | `TXN`                                  |

    
*   若变量类型为 bool 类型，则名称应以 Has, Is, Can 或 Allow 开头
*   变量名更倾向于选择短命名。特别是对于局部变量。 c比lineCount要好，i比sliceIndex要好。
    基本原则是：变量的使用和声明的位置越远，变量名就需要具备越强的描述性

```go
// good
var isExist bool
var hasConflict bool
var canManage bool
var allowGitHook bool
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)

//bad
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```


## 6.1、对于未导出的顶层常量和变量，使用 `_` 作为前缀

对于未导出的全局(包内)`vars`和`consts`， 前面加上前缀`_`，用来明确表示它们是全局符号。

基本依据：顶级变量和常量具有包范围作用域。使用通用名称可能很容易在其他文件中意外使用错误的值

<font color=red>错误类型的变量例外，错误类型变量应以`err`开头</font>。

### 6.1.1、反面示例

```go
// 包内变量
const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)
}
```


### 6.1.2、推荐写法

```go
// 包内变量
const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```


## 6.2、相似声明放一组

`Go` 语言支持将相似的声明放在一个组内。


### 6.2.1、反面示例

```go
// 导入包
import "a"
import "b"
// 常量
const a = 1
const b = 2

var a = 1
var b = 2

type Area float64
type Volume float64
// 不要将不相关的声明放在一组。
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
  EnvVar = "MY_ENV"
)
```


### 6.2.2、推荐写法

```go
// 导入包
import (
  "a"
  "b"
)
// 常量
const (
  a = 1
  b = 2
)

var (
  a = 1
  b = 2
)

type (
  Area float64
  Volume float64
)
// 将相关的声明放在一组
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)
const EnvVar = "MY_ENV"
```


## 6.3、本地变量声明

如果将变量明确设置为某个值，则应使用短变量声明形式 (`:=`)。

```go
// 反面示例
var s = "foo"

// 推荐写法
s := "foo"
```

但是，在某些情况下，`var` 使用关键字时默认值会更清晰。例如，[声明空切片](https://github.com/golang/go/wiki/CodeReviewComments#declaring-empty-slices)。


### 6.6.1、反面示例

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```


### 6.6.2、推荐写法

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```


## 6.4、缩小变量作用域

如果有可能，尽量缩小变量作用范围。除非它与 [减少嵌套](https://github.com/xxjwxc/uber_go_guide_cn#减少嵌套)的规则冲突。

```go
// 反面示例
err := ioutil.WriteFile(name, data, 0644)
if err != nil {
 return err
}

// 推荐写法
if err := ioutil.WriteFile(name, data, 0644); err != nil {
 return err
}
```

# 7、常量命名

*   常量均需遵循驼峰式。
*   如果是枚举类型的常量，需要先创建相应类型：
    ```
    // Scheme 传输协议
    type Scheme string
    
    const (
        // HTTP 表示HTTP明文传输协议
        HTTP Scheme = "http"
        // HTTPS 表示HTTPS加密传输协议
        HTTPS Scheme = "https"
    )
    ```
*   私有全局常量和局部变量规范一致，均以小写字母开头。 `const appVersion = "1.0.0"`


# 7、函数接受变量名

*   短（通常为一个或两个字母长度）
*   类型本身的缩写
*   一致应用于该类型的每个接收器

```
// bad
func (tray Tray)	
func (info *ResearchInfo)	
func (this *ReportWriter)	
func (self *Scanner)	

// good
func (t Tray)
func (ri *ResearchInfo)
func (w *ReportWriter)
func (s *Scanner)
```


# 8、get

Go 不提供对 getter 和 setter 的自动支持。自己提供 getter 和 setter 并没有错，而且这样做通常也是合适的，但将其放入 Getgetter 的名称中既不符合习惯也没有必要。
如果您有一个名为 owner(小写，未导出) 的字段，则 getter 方法应称为Owner(大写，导出)，而不是GetOwner。使用大写名称进行导出提供了区分字段和方法的钩子。
如果需要，setter 函数可能会被称为SetOwner。在实践中，这两个名字读起来都很好：

```
所有者 := obj.Owner()
如果所有者 != 用户 { 
    obj.SetOwner(用户) 
}
```