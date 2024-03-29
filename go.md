go

* package内以大写字母表示可导出的变量或函数，外部均以首字母大写引用；

* Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。

  返回值的名称应当具有一定的意义，它可以作为文档使用。

  没有参数的 `return` 语句返回已命名的返回值。

* 函数外的每个语句都必须以关键字开始（`var`, `func` 等等），也就是不能使用 :=

* 类型有bool、string、（int uint int8 uint8 int16 uint16 int32 uint32 int64 uint64 uintptr）
  byte(uint8)、rune(int32)、（float32 float64）、（complex64 complex128）

* 与 C 不同的是，Go 在不同类型的项之间赋值时需要显式转换。否则直接报错

* for语句可以用作c语言中的for，不加分号则变为c语言中的while。

* if-else，for，switch声明的变量等只在各自的作用域有效

* switch只执行选定case且自动添加break，case值无需为常量，不必为整数。

* defer 语句会将函数推迟到外层函数返回之后执行。(析构函数？？)
  **推迟调用的函数其参数会立即求值**，但直到外层函数返回前该函数都不会被调用。
  多个defer则会进行压栈操作，返回时出栈。

* go没有指针运算。指针引用值时使用点而不是箭头

* 数组赋值 a := [2]int{0, 1}

* 切片赋值 a := []int{0,1,2}，切片不存储任何数据，切片是引用，对切片的修改相当于修改原始数据。
  所有数据类型都可切片，包括struct，或者说能使用数组的地方都能使用切片。从某种程度上，
  切片可以看做C中的指针，不过取值范围有限制；

* len和cap的函数源码？len为当前获取的切片长度，cap表示被切片的(引用对象的)数组长度-起始切片数值

* 切片的扩容，类似于动态扩容数组，为*2，即原来为5扩容后cap为10.

* make函数会分配一个元素为零值的数组并返回一个引用了它的切片

* for的range循环遍历切片，返回值为下标和源切片的**值副本**(值为指针时特别注意)

* map将键映射到值（字典？？）map[key]Type

* 检查map中是否存在某个键，elem, ok = m(key)
  存在则elem返回值，ok为true，不存在elem返回类型0值，ok为flase

* go没有类，可以为结构体类型定义方法；func (v structType) func_name(){}

* 方法即函数；

* 方法与指针重定向；使用指针接收参数，方便对原始数据操作，也少了复制为副本的操作；

* 函数传参、方法类型参数，值为副本，除非使用指针才能修改原始值；

* **接口类型** interface是由一组方法签名定义的集合。type A interface{func1() string}
  接口类型的变量可以保存任何实现了这些方法的值。（多态？？）
  A的赋值严格遵循值和指针赋值（区别于方法）
  调用接口，看做将执行type的方法并且将value传递给该方法
type A int     var i interface{} = A(5)
  
* 接口也是值，意味着也可以作为参数或返回值；
  在内部，接口值可以看做包含值和具体类型的元组：(value, type)

* 接口隐式声明，如果类型A实现了接口B的方法C，那么在var对变量赋值时，
  可以不必使用接口B类型就可直接访问类型A的接口B方法C
type A int；type B interface{C()}；func (a A) C() {}；
  那么：var i B = A() 和 var i = A() 效果一样，都能使用 i.C()
  
* 调用nil接口的方法会出发空指针错误，类型并未赋值就直接给接口；但接口本身并不是nil，毕竟有值；

* 空接口可以保存任意类型的值；但是因为空接口中没有方法，那么接口也就不能调用方法；

* 类型断言，提供访问接口值底层具体值的方式，元组类型访问；v, ok := i.(T)，
  存在T类型，v为元组value，ok为true；不存在T类型，v为T类型0值，ok为false；
  只需要值时可以不用ok接收(v := i.(T))，但是不存在T类型时会触发panic；与map检查键是否存在相似；

* 类型选择，是一种按顺序从几个类型断言中选择分支的结构。
  与switch相似，但是case为类型而非值；switch v := i.(**type**){case T1:  case T2: default:  }
  此处type为关键字type
  未匹配的情况，v与i的元组相同（类型和值）

* string() string 自定义定义printf方法

* error类型是一个内建接口，意味着也可以自定义方法；nil表示成功，非nil表示失败

* goroutine  go线程

* 信道是带有类型的管道，你可以通过它用信道操作符`<-` 来发送或者接收值。箭头表示数据流通的方向
  信道与map和silence一样，使用前必须先创建 ch := make(chan int)
  默认情况下，发送和接收操作在另一端准备好之前都会阻塞。
  这使得 Go 程可以在没有显式的锁或竞态变量的情况下进行同步。

* 带缓冲信道 ch := make(chan int, 100)
  当缓冲区满时，向ch发送数据阻塞，当缓冲区空时，从ch获取数据阻塞

* 使用close关闭信道，必须由发送方关闭，range ch会一直从信道中取值直到信道关闭
  使用 v, ok := <-ch 来判断ch是否关闭(ok为false)

