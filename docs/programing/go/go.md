# 安装go

check go installed
`go version`

gopath: /Users/name/go

`cd $GOPATH
mkdir bin
mkdir src
mkdir pkg`

IDE: Visual Studio Code

IDE Plugin: go

# First Round
https://learn.microsoft.com/zh-cn/training/modules/go-get-started/2-install-go?pivots=macos

hello world
```
package main

import "fmt"

func main() {
    fmt.Println("Hello World!")
}
```

`go run main.go` -> Hello World
`go build main.go` -> executable file

变量
```
var firstName string

var firstName, lastName string

var age int

var (
    firstName, lastName string
    age int
)

var (
    firstName string = "John"
    lastName  string = "Doe"
    age       int    = 32
)

var (
    firstName, lastName, age = "John", "Doe", 32
)

// mostly used 
func main() {
    firstName, lastName := "John", "Doe"
    age := 32
    fmt.Println(firstName, lastName, age)
}


```

常量
```

const HTTPStatusOK = 200
const (
    StatusOK              = 0
    StatusConnectionReset = 1
    StatusOtherError      = 2
)
```

Go 有四类数据类型：
基本类型：数字、字符串和布尔值
聚合类型：数组和结构
引用类型：指针、切片、映射、函数和通道
接口类型：接口

数字
```
var integer32 int = 2147483648

var integer8 int8 = 127
var integer16 int16 = 32767
var integer32 int32 = 2147483647
var integer64 int64 = 9223372036854775807

var float32 float32 = 2147483647
var float64 float64 = 9223372036854775807

```

布尔, true or false, cannot be 1 or 0
`var featureFlag bool = true`

字符串
```go
var firstName string = "John"
lastName := "Doe"

/*
\n：新行
\r：回车符
\t：制表符
\'：单引号
\"：双引号
\\：反斜杠
*/

fullName := "John Doe \t(alias \"Foo\")\n"
```

类型转换
```commandline
var integer16 int16 = 127
var integer32 int32 = 32767
fmt.Println(int32(integer16) + integer32)
```

```
package main

import (
    "fmt"
    "strconv"
)

func main() {
    i, _ := strconv.Atoi("-42")
    s := strconv.Itoa(-42)
    fmt.Println(i, s)
}
```

函数
```

func name(parameters) (results) {
    body-content
}
```

```
package main

import (
    "fmt"
    "os"
    "strconv"
)

func main() {
    sum := sum(os.Args[1], os.Args[2])
    fmt.Println("Sum:", sum)
}

func sum(number1 string, number2 string) int {
    int1, _ := strconv.Atoi(number1)
    int2, _ := strconv.Atoi(number2)
    return int1 + int2
}

```

```
func calc(number1 string, number2 string) (sum int, mul int) {
    int1, _ := strconv.Atoi(number1)
    int2, _ := strconv.Atoi(number2)
    sum = int1 + int2
    mul = int1 * int2
    return
}
```

在 Go 中，有两个运算符可用于处理指针：
- `&` 运算符使用其后对象的地址。
- `*` 运算符取消引用指针。 你可以前往指针中包含的地址访问其中的对象。
```

package main

import "fmt"

func main() {
    firstName := "John"
    updateName(&firstName)
    fmt.Println(firstName)
}

func updateName(name *string) {
    *name = "David"
}
```

包
```
import "math/cmplx"

// 包名.X
cmplx.Inf()
```

创建包
```
// 目录 
src/
  calculator/
    sum.go
```

```
// sum.go   
package calculator

var logMessage = "[LOG]"

// Version of the calculator
var Version = "1.0"

func internalSum(number int) int {
    return number - 1
}

// Sum two integer numbers
func Sum(number1, number2 int) int {
    return number1 + number2
}
```
创建模块
`go mod init github.com/myuser/calculator`

```
目录
src/
  calculator/
    go.mod
    sum.go
```

引用包
告诉Go你会引用包
`go mod init helloworld`

go.mod
```

module helloworld

go 1.14

require github.com/myuser/calculator v0.0.0

replace github.com/myuser/calculator => ../calculator
```

引用外部（第三方）包
```

package main

import (
    "fmt"
    "github.com/myuser/calculator"
    "rsc.io/quote"
)

func main() {
    total := calculator.Sum(3, 5)
    fmt.Println(total)
    fmt.Println("Version: ", calculator.Version)
    fmt.Println(quote.Hello())
}

```

```

// may need retry
go get rsc.io/quote
go run main.go
```

控制流
```

package main

import "fmt"

func main() {
    x := 27
    if x%2 == 0 {
        fmt.Println(x, "is even")
    }
}
```

```

package main

import "fmt"

func givemeanumber() int {
    return -1
}

func main() {
    // 在 Go 中，在 if 块内声明变量是惯用的方式
    if num := givemeanumber(); num < 0 {
        fmt.Println(num, "is negative")
    } else if num < 10 {
        fmt.Println(num, "has only one digit")
    } else {
        fmt.Println(num, "has multiple digits")
    }
}
```

switch
```

package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    sec := time.Now().Unix()
    rand.Seed(sec)
    i := rand.Int31n(10)

    switch i {
    case 0:
        fmt.Print("zero...")
    case 1:
        fmt.Print("one...")
    case 2:
        fmt.Print("two...")
    }

    fmt.Println("ok")
}
```

```
package main

import "fmt"

func location(city string) (string, string) {
    var region string
    var continent string
    switch city {
    case "Delhi", "Hyderabad", "Mumbai", "Chennai", "Kochi":
        region, continent = "India", "Asia"
    case "Lafayette", "Louisville", "Boulder":
        region, continent = "Colorado", "USA"
    case "Irvine", "Los Angeles", "San Diego":
        region, continent = "California", "USA"
    default:
        region, continent = "Unknown", "Unknown"
    }
    return region, continent
}
func main() {
    region, continent := location("Irvine")
    fmt.Printf("John works in %s, %s\n", region, continent)
}
```

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    r := rand.Float64()
    // 不带条件
    switch {
    case r > 0.1:
        fmt.Println("Common case, 90% of the time")
    default:
        fmt.Println("10% of the time")
    }
}
```

```
// Go 中，当逻辑进入某个 case 时，它会退出 switch 块，除非你显式停止它。 若要使逻辑进入到下一个紧邻的 case，请使用 fallthrough 关键字。
package main

import (
    "fmt"
)

func main() {
    switch num := 15; {
    case num < 50:
        fmt.Printf("%d is less than 50\n", num)
        fallthrough
    case num > 100:
        fmt.Printf("%d is greater than 100\n", num)
        fallthrough
    case num < 200:
        fmt.Printf("%d is less than 200", num)
    }
}
```

for
```
func main() {
    sum := 0
    for i := 1; i <= 100; i++ {
        sum += i
    }
    fmt.Println("sum of 1..100 is", sum)
}
```

```
// like while

package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    var num int64
    rand.Seed(time.Now().Unix())
    for num != 5 {
        num = rand.Int63n(15)
        fmt.Println(num)
    }
}
```

```
// break
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    var num int32
    sec := time.Now().Unix()
    rand.Seed(sec)

    for {
        fmt.Print("Writing inside the loop...")
        if num = rand.Int31n(10); num == 5 {
            fmt.Println("finish!")
            break
        }
        fmt.Println(num)
    }
}
```
```
//continue

package main

import "fmt"

func main() {
    sum := 0
    for num := 1; num <= 100; num++ {
        if num%5 == 0 {
            continue
        }
        sum += num
    }
    fmt.Println("The sum of 1 to 100, but excluding numbers divisible by 5, is", sum)
}
```

Go 特有的控制流， defer panic and recover

defer 语句会推迟函数（包括任何参数）的运行，直到包含 defer 语句的函数完成。 通常情况下，当你想要避免忘记任务（例如关闭文件或运行清理进程）时，可以推迟某个函数的运行。

```
package main

import "fmt"

func main() {
    for i := 1; i <= 4; i++ {
        defer fmt.Println("deferred", -i)
        fmt.Println("regular", i)
    }
}
```

``` output
regular 1
regular 2
regular 3
regular 4
deferred -4
deferred -3
deferred -2
deferred -1
```

```
// defer 
package main

import (
    "io"
    "os"
    "fmt"
)

func main() {
    newfile, error := os.Create("learnGo.txt")
    if error != nil {
        fmt.Println("Error: Could not create file.")
        return
    }
    defer newfile.Close()

    if _, error = io.WriteString(newfile, "Learning Go!"); error != nil {
	    fmt.Println("Error: Could not write to file.")
        return
    }

    newfile.Sync()
}
```
内置 panic() 函数可以停止 Go 程序中的正常控制流。 当你使用 panic 调用时，任何延迟的函数调用都将正常运行。 进程会在堆栈中继续，直到所有函数都返回。 然后，程序会崩溃并记录日志消息。 此消息包含错误信息和堆栈跟踪，有助于诊断问题的根本原因。
调用 panic() 函数时，可以添加任何值作为参数。 通常，你会发送一条错误消息，说明为什么会进入紧急状态。

```
package main

import "fmt"

func highlow(high int, low int) {
    if high < low {
        fmt.Println("Panic!")
        panic("highlow() low greater than high")
    }
    defer fmt.Println("Deferred: highlow(", high, ",", low, ")")
    fmt.Println("Call: highlow(", high, ",", low, ")")

    highlow(high, low + 1)
}

func main() {
    highlow(2, 0)
    fmt.Println("Program finished successfully!")
}
```

```
Call: highlow( 2 , 0 )
Call: highlow( 2 , 1 )
Call: highlow( 2 , 2 )
Panic!
Deferred: highlow( 2 , 2 )
Deferred: highlow( 2 , 1 )
Deferred: highlow( 2 , 0 )
panic: highlow() low greater than high

goroutine 1 [running]:
main.highlow(0x2, 0x3)
	/tmp/sandbox/prog.go:13 +0x34c
main.highlow(0x2, 0x2)
	/tmp/sandbox/prog.go:18 +0x298
main.highlow(0x2, 0x1)
	/tmp/sandbox/prog.go:18 +0x298
main.highlow(0x2, 0x0)
	/tmp/sandbox/prog.go:18 +0x298
main.main()
	/tmp/sandbox/prog.go:6 +0x37

Program exited: status 2.
```
在发生未预料到的严重错误时，系统通常会运行对 panic() 函数的调用。 若要避免程序崩溃，可以使用名为 recover() 的另一个函数。
panic 和 recover 函数的组合是 Go 处理异常的惯用方式。 其他编程语言使用 try/catch 块。 
```
func main() {
    defer func() {
	    handler := recover()
        if handler != nil {
            fmt.Println("main(): recover", handler)
        }
    }()

    highlow(2, 0)
    fmt.Println("Program finished successfully!")
}
```

数组
```
package main

import "fmt"

func main() {
    var a [3]int
    a[1] = 10
    fmt.Println(a[0])
    fmt.Println(a[1])
    fmt.Println(a[len(a)-1])
}
```
```
package main

import "fmt"

func main() {
    cities := [5]string{"New York", "Paris", "Berlin", "Madrid"}
    fmt.Println("Cities:", cities)
}
```
```
// 不知道多少个位置
q := [...]int{1, 2, 3}
```

```
// 在第99个位置上指定了值
package main

import "fmt"

func main() {
    numbers := [...]int{99: -1}
    fmt.Println("First Position:", numbers[0])
    fmt.Println("Last Position:", numbers[99])
    fmt.Println("Length:", len(numbers))
}
```
```
package main

import "fmt"

func main() {
    var twoD [3][5]int
    for i := 0; i < 3; i++ {
        for j := 0; j < 5; j++ {
            twoD[i][j] = (i + 1) * (j + 1)
        }
        fmt.Println("Row", i, twoD[i])
    }
    fmt.Println("\nAll at once:", twoD)
}
```
切片, 3 个组件：
- 指向基础数组中第一个可访问元素的指针。 此元素不一定是数组的第一个元素 array[0]。
- 切片的长度。 切片中的元素数目。
- 切片的容量。 切片开头与基础数组结束之间的元素数目。
```
package main

import "fmt"

func main() {
    months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
    fmt.Println(months)
    fmt.Println("Length:", len(months))
    fmt.Println("Capacity:", cap(months))
}
```
```go
package main

import "fmt"

func main() {
    months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
    quarter1 := months[0:3]
    quarter2 := months[3:6]
    quarter3 := months[6:9]
    quarter4 := months[9:12]
    fmt.Println(quarter1, len(quarter1), cap(quarter1))
    fmt.Println(quarter2, len(quarter2), cap(quarter2))
    fmt.Println(quarter3, len(quarter3), cap(quarter3))
    fmt.Println(quarter4, len(quarter4), cap(quarter4))
}
```
请注意，切片的长度不变，但容量不同。 我们来了解 quarter2 切片。 切片长度为 3，但容量为 9，原因是基础数组有更多元素或位置可供使用，但对切片而言不可见。
切片容量仅指出切片可扩展的程度
```go

package main

import "fmt"

func main() {
    months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
    quarter2 := months[3:6]
    quarter2Extended := quarter2[:4]
    fmt.Println(quarter2, len(quarter2), cap(quarter2))
    fmt.Println(quarter2Extended, len(quarter2Extended), cap(quarter2Extended))
}
```
```
[April May June] 3 9
[April May June July] 4 9
```

append, append(slice, element)
```
package main

import "fmt"

func main() {
    var numbers []int
    for i := 0; i < 10; i++ {
        numbers = append(numbers, i)
        fmt.Printf("%d\tcap=%d\t%v\n", i, cap(numbers), numbers)
    }
}
```
删除
```
package main

import "fmt"

func main() {
    letters := []string{"A", "B", "C", "D", "E"}
    remove := 2

	if remove < len(letters) {

		fmt.Println("Before", letters, "Remove ", letters[remove])

		letters = append(letters[:remove], letters[remove+1:]...)

		fmt.Println("After", letters)
	}

}
```

切片副本
copy(dst, src []Type)
```
// 初始化一个[]string
slice2 := make([]string, 3)
copy(slice2, letters[1:4])
```

```
package main

import "fmt"

func main() {
    letters := []string{"A", "B", "C", "D", "E"}
    fmt.Println("Before", letters)

    slice1 := letters[0:2]
    slice2 := letters[1:4]

    slice1[1] = "Z"

    fmt.Println("After", letters)
    fmt.Println("Slice2", slice2)
}
```
```
Before [A B C D E]
After [A Z C D E]
Slice2 [Z C D]
```
请注意对 slice1 所做的更改如何影响 letters 数组和 slice2。 可在输出中看到字母 B 已替换为 Z，它会影响指向 letters 数组的每个切片。
```
package main

import "fmt"

func main() {
    letters := []string{"A", "B", "C", "D", "E"}
    fmt.Println("Before", letters)

    slice1 := letters[0:2]

    slice2 := make([]string, 3)
    copy(slice2, letters[1:4])

    slice1[1] = "Z"

    fmt.Println("After", letters)
    fmt.Println("Slice2", slice2)
}
```

map
```
package main

import "fmt"

func main() {
    studentsAge := map[string]int{
        "john": 32,
        "bob":  31,
    }
    fmt.Println(studentsAge)
}
```
```
// 初始化map
studentsAge := make(map[string]int)
```
```
package main

import "fmt"

func main() {
    studentsAge := make(map[string]int)
    // 添加项
    studentsAge["john"] = 32
    studentsAge["bob"] = 31
    fmt.Println(studentsAge)
    
    // 访问项
    fmt.Println("Bob's age is", studentsAge["bob"])
    
    // 删除项
    delete(studentsAge, "john")
    fmt.Println(studentsAge)
    
    // 循环
    for name, age := range studentsAge {
        fmt.Printf("%s\t%d\n", name, age)
    }
}
```

struct
```
type Employee struct {
    ID        int
    FirstName string
    LastName  string
    Address   string
}
```
```
// 可像操作其他类型一样使用新类型声明一个变量
var john Employee

// 一边声明，一边初始化
employee := Employee{1001, "John", "Doe", "Doe's Street"}

employee := Employee{LastName: "Doe", FirstName: "John"}

employee.ID = 1001
fmt.Println(employee.FirstName)
```

可使用 & 运算符生成指向结构的指针
```
package main

import "fmt"

type Employee struct {
    ID        int
    FirstName string
    LastName  string
    Address   string
}

func main() {
    employee := Employee{LastName: "Doe", FirstName: "John"}
    fmt.Println(employee)
    employeeCopy := &employee
    employeeCopy.FirstName = "David"
    fmt.Println(employee)
}
```

```
// output
{0 John Doe }
{0 David Doe }
```

结构嵌入
```
type Person struct {
    ID        int
    FirstName string
    LastName  string
    Address   string
}

```
```
type Employee struct {
    Information Person
    ManagerID   int
}

var employee Employee
// Information
employee.Information.FirstName = "John"
```

```
package main

import "fmt"

type Person struct {
    ID        int
    FirstName string
    LastName  string
    Address   string
}

// User Person directly
type Employee struct {
    Person
    ManagerID int
}

type Contractor struct {
    Person
    CompanyID int
}

func main() {
    employee := Employee{
        Person: Person{
            FirstName: "John",
        },
    }
    // employ.LastName instead of employee.person.name
    employee.LastName = "Doe"
    fmt.Println(employee.FirstName)
}
```

JSON 编码解码
```
假设你不希望 JSON 输出显示 FirstName 而只显示 name，或者忽略空字段， 可使用如下例所示的字段标记：
type Person struct {
    ID        int    
    FirstName string `json:"name"`
    LastName  string
    Address   string `json:"address,omitempty"`
}
```

若要将结构编码为 JSON，请使用 json.Marshal 函数
```go
package main

import (
    "encoding/json"
    "fmt"
)

type Person struct {
    ID        int
    FirstName string `json:"name"`
    LastName  string
    Address   string `json:"address,omitempty"`
}

type Employee struct {
    Person
    ManagerID int
}

type Contractor struct {
    Person
    CompanyID int
}

func main() {
    employees := []Employee{
        Employee{
            Person: Person{
                LastName: "Doe", FirstName: "John",
            },
        },
        Employee{
            Person: Person{
                LastName: "Campbell", FirstName: "David",
            },
        },
    }

    data, _ := json.Marshal(employees)
    fmt.Printf("%s\n", data)

    var decoded []Employee
    json.Unmarshal(data, &decoded)
    fmt.Printf("%v", decoded)
}
```

错误处理
Go 的错误处理方法只是一种只需要 if 和 return 语句的控制流机制。
```
employee, err := getInformation(1000)
if err != nil {
    // Something is wrong. Do something.
}
```

```
package main

import (
    "fmt"
    "os"
)

type Employee struct {
    ID        int
    FirstName string
    LastName  string
    Address   string
}

func main() {
    employee, err := getInformation(1001)
    if err != nil {
        // Something is wrong. Do something.
    } else {
        fmt.Print(employee)
    }
}

func getInformation(id int) (*Employee, error) {
    employee, err := apiCallEmployee(1000)
    if err != nil {
        return nil, fmt.Errorf("Got an error when getting the employee information: %v", err)
    }
    return employee, nil
}

func apiCallEmployee(id int) (*Employee, error) {
    employee := Employee{LastName: "Doe", FirstName: "John"}
    return &employee, nil
}
```
暂时性错误，重试机制
```
func getInformation(id int) (*Employee, error) {
    for tries := 0; tries < 3; tries++ {
        employee, err := apiCallEmployee(1000)
        if err == nil {
            return employee, nil
        }

        fmt.Println("Server is not responding, retrying ...")
        time.Sleep(time.Second * 2)
    }

    return nil, fmt.Errorf("server has failed to respond to get the employee information")
}
```
// 创建可重用的Error
```
var ErrNotFound = errors.New("Employee not found!")

func getInformation(id int) (*Employee, error) {
    if id != 1001 {
        return nil, ErrNotFound
    }

    employee := Employee{LastName: "Doe", FirstName: "John"}
    return &employee, nil
}
```

error.Is
```
employee, err := getInformation(1000)
if errors.Is(err, ErrNotFound) {
    fmt.Printf("NOT FOUND: %v\n", err)
} else {
    fmt.Print(employee)
}
```

log 包
```go
package main

import (
    "fmt"
    "log"
)

func main() {
    log.Print("Hey, I'm a log!")
    
}
```
```go
2020/12/19 13:39:17 Hey, I'm a log!

```
```
log.Fatal("Hey, I'm an error log!")
fmt.Print("Can you see me?")
```
```
2020/12/19 13:53:19  Hey, I'm an error log!
exit status 1
```

```
package main

import (
    "fmt"
    "log"
)

func main() {
    log.Panic("Hey, I'm an error log!")
    fmt.Print("Can you see me?")
}
```
```go
2020/12/19 13:53:19  Hey, I'm an error log!
panic: Hey, I'm an error log!

goroutine 1 [running]:
log.Panic(0xc000060f58, 0x1, 0x1)
        /usr/local/Cellar/go/1.15.5/libexec/src/log/log.go:351 +0xae
main.main()
        /Users/christian/go/src/helloworld/logs.go:9 +0x65
exit status 2
```

```go
package main

import (
    "log"
)

func main() {
    log.SetPrefix("main(): ")
    log.Print("Hey, I'm a log!")
    log.Fatal("Hey, I'm an error log!")
}
```
```
main(): 2021/01/05 13:59:58 Hey, I'm a log!
main(): 2021/01/05 13:59:58 Hey, I'm an error log!
exit status 1
```
log输出到文件
```go
package main

import (
    "log"
    "os"
)

func main() {
    file, err := os.OpenFile("info.log", os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        log.Fatal(err)
    }

    defer file.Close()

    log.SetOutput(file)
    log.Print("Hey, I'm a log!")
}
```

Go 的几个记录框架有 Logrus、zerolog、zap 和 Apex。
`go get -u github.com/rs/zerolog/log`

```
package main

import (
    "github.com/rs/zerolog"
    "github.com/rs/zerolog/log"
)

func main() {
    zerolog.TimeFieldFormat = zerolog.TimeFormatUnix
    log.Print("Hey! I'm a log message!")
}
```
`{"level":"debug","time":1609855453,"message":"Hey! I'm a log message!"}`

你可以使用 zerolog 实现其他功能，例如使用分级的日志记录、使用格式化的堆栈跟踪，以及使用多个记录器实例来管理不同输出。

方法
```go
func (variable type) MethodName(parameters ...) {
    // method functionality
}
```

```go
package main

import "fmt"

type triangle struct {
    size int
}

type square struct {
    size int
}

func (t triangle) perimeter() int {
    return t.size * 3
}

func (s square) perimeter() int {
    return s.size * 4
}

func main() {
    t := triangle{3}
    s := square{4}
    fmt.Println("Perimeter (triangle):", t.perimeter())
    fmt.Println("Perimeter (square):", s.perimeter())
}
```

方法中的指针
```go
func (t *triangle) doubleSize() {
    t.size *= 2
}
func main() {
    t := triangle{3}
    t.doubleSize()
    fmt.Println("Size:", t.size)
    fmt.Println("Perimeter:", t.perimeter())
}
```

```go
package main

import (
    "fmt"
    "strings"
)

type upperstring string

func (s upperstring) Upper() string {
    return strings.ToUpper(string(s))
}

func main() {
    s := upperstring("Learning Go!")
    fmt.Println(s)
    fmt.Println(s.Upper())
}
```
函数(function)是一段代码，通过名字来进行调用。它能将一些数据（参数）传递进去进行处理，然后返回一些数据（返回值），也可以没有返回值。

所有传递给函数的数据都是显式传递的。

方法(method)也是一段代码，也通过名字来进行调用，但它跟一个对象相关联。方法和函数大致上是相同的，但有两个主要的不同之处：
- 方法中的数据是隐式传递的；
- 方法可以操作类内部的数据（请记住，对象是类的实例化–类定义了一个数据类型，而对象是该数据类型的一个实例化）

方法重载
```go
type coloredTriangle struct {
    triangle
    color string
}

func main() {
    t := coloredTriangle{triangle{3}, "blue"}
    fmt.Println("Size:", t.size)
    fmt.Println("Perimeter", t.perimeter())
}
```

Go 中的接口类似于蓝图。 一种抽象类型，只包括具体类型必须拥有或实现的方法。
```go
type Shape interface {
    Perimeter() float64
    Area() float64
}
```

实现
```go
type Square struct {
    size float64
}

func (s Square) Area() float64 {
    return s.size * s.size
}

func (s Square) Perimeter() float64 {
    return s.size * 4
}

func main() {
    var s Shape = Square{3}
    fmt.Printf("%T\n", s)
    fmt.Println("Area: ", s.Area())
    fmt.Println("Perimeter:", s.Perimeter())
}
```

实现字符串接口
```go
type Stringer interface {
    String() string
}

// fmt.Printf 函数使用此接口来输出值，这意味着你可以编写自定义 String() 方法来打印自定义字符串
package main

import "fmt"

type Person struct {
    Name, Country string
}

func (p Person) String() string {
    return fmt.Sprintf("%v is from %v", p.Name, p.Country)
}
func main() {
    rs := Person{"John Doe", "USA"}
    ab := Person{"Mark Collins", "United Kingdom"}
    // %s call String()
    fmt.Printf("%s\n%s\n", rs, ab)
}
```

扩展现有实现
```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    resp, err := http.Get("https://api.github.com/users/microsoft/repos?page=15&per_page=5")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(1)
    }

    io.Copy(os.Stdout, resp.Body)
}
```
程序终版
1. 定义一个struct
2. 根据struct写新方法
3. 调用新方法
```go
package main

import (
    "encoding/json"
    "fmt"
    "io"
    "net/http"
    "os"
)

type GitHubResponse []struct {
    FullName string `json:"full_name"`
}

type customWriter struct{}

func (w customWriter) Write(p []byte) (n int, err error) {
    var resp GitHubResponse
    json.Unmarshal(p, &resp)
    for _, r := range resp {
        fmt.Println(r.FullName)
    }
    return len(p), nil
}

func main() {
    resp, err := http.Get("https://api.github.com/users/microsoft/repos?page=15&per_page=5")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(1)
    }

    writer := customWriter{}
    io.Copy(writer, resp.Body)
}
```

编写自定义服务器API
我们一起来探讨接口的另一种用例，如果你要创建服务器 API，你可能会发现此用例非常实用。 编写 Web 服务器的常用方式是使用 net/http 程序包中的 http.Handler 接口
```go
package http

type Handler interface {
    ServeHTTP(w ResponseWriter, r *Request)
}

func ListenAndServe(address string, h Handler) error
```

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

type dollars float32

func (d dollars) String() string {
    return fmt.Sprintf("$%.2f", d)
}

type database map[string]dollars

func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    for item, price := range db {
        fmt.Fprintf(w, "%s: %s\n", item, price)
    }
}

func main() {
    db := database{"Go T-Shirt": 25, "Go Jacket": 55}
    log.Fatal(http.ListenAndServe("localhost:8000", db))
}
```

go 并发，通过goroutine 和channel

```go
func main(){
    login()
    go launch()
}
```

```go
// 匿名函数
func main(){
    login()
    go func() {
        launch()
    }()
}
```

```go

package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    start := time.Now()

    apis := []string{
        "https://management.azure.com",
        "https://dev.azure.com",
        "https://api.github.com",
        "https://outlook.office.com/",
        "https://api.somewhereintheinternet.com/",
        "https://graph.microsoft.com",
    }
    // 普通模式
    for _, api := range apis {
        _, err := http.Get(api)
        if err != nil {
            fmt.Printf("ERROR: %s is down!\n", api)
            continue
        }

        fmt.Printf("SUCCESS: %s is up and running!\n", api)
    }

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}
```

```go
// 写个方法供调用
func checkAPI(api string) {
    _, err := http.Get(api)
    if err != nil {
        fmt.Printf("ERROR: %s is down!\n", api)
        return
    }

    fmt.Printf("SUCCESS: %s is up and running!\n", api)
}

// main函数中
for _, api := range apis {
    go checkAPI(api)
}

time.Sleep(3 * time.Second)

```

channel通信机制
`ch := make(chan int)`

一个 channel 可以执行两项操作：发送数据和接收数据。 若要指定 channel 具有的操作类型，需要使用 channel 运算符 <-。 此外，在 channel 中发送数据和接收数据属于阻止操作。
```go
ch <- x // sends (or writes ) x through channel ch
x = <-ch // x receives (or reads) data sent to the channel ch
<-ch // receives data, but the result is discarded
```
`close(ch)`

```go
func checkAPI(api string, ch chan string) {
    _, err := http.Get(api)
    if err != nil {
        ch <- fmt.Sprintf("ERROR: %s is down!\n", api)
        return
    }

    ch <- fmt.Sprintf("SUCCESS: %s is up and running!\n", api)
}
```

```
ch := make(chan string)

for _, api := range apis {
    go checkAPI(api, ch)
}

fmt.Print(<-ch)
```


```
// range 用法
package main
import "fmt"

func main() {
    x := []string{"a", "b", "c"}
    for v := range x {
    fmt.Println(v) //prints 0, 1, 2
    }
    for _, v := range x {
    fmt.Println(v) //prints a, b, c
    }
}
```

无缓冲channel, 默认是无缓冲的，没有接收就阻止发送
```
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    start := time.Now()

    apis := []string{
        "https://management.azure.com",
        "https://dev.azure.com",
        "https://api.github.com",
        "https://outlook.office.com/",
        "https://api.somewhereintheinternet.com/",
        "https://graph.microsoft.com",
    }

    ch := make(chan string)

    for _, api := range apis {
        go checkAPI(api, ch)
    }
    // 读取操作需要和发送操作数量一致，接收>发送程序不能正常终止，接收<发送程序会提前终止
    for i := 0; i < len(apis); i++ {
        fmt.Print(<-ch)
    }

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}

func checkAPI(api string, ch chan string) {
    _, err := http.Get(api)
    if err != nil {
        ch <- fmt.Sprintf("ERROR: %s is down!\n", api)
        return
    }

    ch <- fmt.Sprintf("SUCCESS: %s is up and running!\n", api)
}
```

有缓冲channel, 类似队列。
每次向 channel 发送数据时，都会将元素添加到队列中。 然后，接收操作将从队列中删除该元素。 当 channel 已满时，任何发送操作都将等待，直到有空间保存数据。 相反，如果 channel 是空的且存在读取操作，程序则会被阻止，直到有数据要读取。
```
ch := make(chan string, 10)
```

```
package main

import (
    "fmt"
)

func send(ch chan string, message string) {
    ch <- message
}

func main() {
    size := 4
    ch := make(chan string, size)
    send(ch, "one")
    send(ch, "two")
    send(ch, "three")
    send(ch, "four")
    fmt.Println("All data sent to the channel ...")

    for i := 0; i < size; i++ {
        fmt.Println(<-ch)
    }

    fmt.Println("Done!")
}
```
Output
```
All data sent to the channel ...
one
two
three
four
Done!
```
而如果改变size为2则会报错，因为send直接将内容放进channel, 它没有创建goroutine, 所以没有排队的操作。
channel 与 goroutine 有着紧密的联系。 如果没有另一个 goroutine 从 channel 接收数据，则整个程序可能会永久处于被阻止状态。 正如你所见，这种情况确实会发生。
我们建议在使用 channel 时始终使用 goroutine。


可以更改 channel 的大小，用更小或更大的数字进行测试，程序仍能正常运行:
```
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    start := time.Now()

    apis := []string{
        "https://management.azure.com",
        "https://dev.azure.com",
        "https://api.github.com",
        "https://outlook.office.com/",
        "https://api.somewhereintheinternet.com/",
        "https://graph.microsoft.com",
    }

    ch := make(chan string, 10)

    for _, api := range apis {
        go checkAPI(api, ch)
    }

    for i := 0; i < len(apis); i++ {
        fmt.Print(<-ch)
    }

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}

func checkAPI(api string, ch chan string) {
    _, err := http.Get(api)
    if err != nil {
        ch <- fmt.Sprintf("ERROR: %s is down!\n", api)
        return
    }

    ch <- fmt.Sprintf("SUCCESS: %s is up and running!\n", api)
}
```

在使用通道作为函数的参数时，可以指定通道是要“发送”数据还是“接收”数据。 随着程序的增长，可能会使用大量的函数，这时候，最好记录每个 channel 的意图，以便正确使用它们
```
chan<- int // it's a channel to only send data
<-chan int // it's a channel to only receive data
```

```
package main

import "fmt"

func send(ch chan<- string, message string) {
    fmt.Printf("Sending: %#v\n", message)
    ch <- message
}

func read(ch <-chan string) {
    fmt.Printf("Receiving: %#v\n", <-ch)
}

func main() {
    ch := make(chan string, 1)
    send(ch, "Hello World!")
    read(ch)
}
```
多路复用
有时，在使用多个 channel 时，需要等待事件发生。 例如，当程序正在处理的数据中出现异常时，可以包含一些逻辑来取消操作。
select 语句的工作方式类似于 switch 语句，但它适用于 channel。 它会阻止程序的执行，直到它收到要处理的事件。 如果它收到多个事件，则会随机选择一个。
select 语句的一个重要方面是，它在处理事件后完成执行。 如果要等待更多事件发生，则可能需要使用循环。
让我们使用以下程序来看看 select 的运行情况：

```
package main

import (
    "fmt"
    "time"
)

func process(ch chan string) {
    time.Sleep(3 * time.Second)
    ch <- "Done processing!"
}

func replicate(ch chan string) {
    time.Sleep(1 * time.Second)
    ch <- "Done replicating!"
}

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go process(ch1)
    go replicate(ch2)

    for i := 0; i < 2; i++ {
        select {
        case process := <-ch1:
            fmt.Println(process)
        case replicate := <-ch2:
            fmt.Println(replicate)
        }
    }
}

```

```
// 输出
Done replicating!
Done processing!
```

请注意，replicate 函数首先完成，这就是首先在终端中看到其输出的原因。 main 函数存在一个循环，因为 select 语句在收到事件后立即结束，但我们仍在等待 process 函数完成。??

单线程计算斐波那契数
```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func fib(number float64) float64 {
    x, y := 1.0, 1.0
    for i := 0; i < int(number); i++ {
        x, y = y, x+y
    }

    r := rand.Intn(3)
    time.Sleep(time.Duration(r) * time.Second)

    return x
}

func main() {
    start := time.Now()

    for i := 1; i < 15; i++ {
        n := fib(float64(i))
    fmt.Printf("Fib(%v): %v\n", i, n)
    }

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}
```

并发计算
```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func fib(number float64, ch chan string) {
    x, y := 1.0, 1.0
    for i := 0; i < int(number); i++ {
        x, y = y, x+y
    }

    r := rand.Intn(3)
    time.Sleep(time.Duration(r) * time.Second)

    ch <- fmt.Sprintf("Fib(%v): %v\n", number, x)
}

func main() {
    start := time.Now()

    size := 15
    ch := make(chan string, size)

    for i := 0; i < size; i++ {
        go fib(float64(i), ch)
    }

    for i := 0; i < size; i++ {
        fmt.Printf(<-ch)
    }

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}
```

使用两个无缓冲channel
```
package main

import (
    "fmt"
    "time"
)

var quit = make(chan bool)

func fib(c chan int) {
    x, y := 1, 1

    for {
        select {
            case c <- x:
                x, y = y, x+y
            case <-quit:
                fmt.Println("Done calculating Fibonacci!")
            return
        }
    }
}

func main() {
    start := time.Now()

    command := ""
    data := make(chan int)

    go fib(data)

    for {
        num := <-data
        fmt.Println(num)
        fmt.Scanf("%s", &command)
        if command == "quit" {
            quit <- true
            break
        }
    }

    time.Sleep(1 * time.Second)

    elapsed := time.Since(start)
    fmt.Printf("Done! It took %v seconds!\n", elapsed.Seconds())
}
```

test

```
package bank

import "testing"

func TestAccount(t *testing.T) {

}
```

Terminal run `go test -v`
Go 将查找所有 *_test.go 文件来运行测试

TODO: write bank project in training course

Start next time:
https://learn.microsoft.com/zh-cn/training/modules/go-write-test-program/1-project-files