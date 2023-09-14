# 只有实际代码使用接口时才会检查接口语法正确性
```go
package main

import "fmt"

type People interface {
	Speak(string) string
}

type Student struct{}

func (stu *Student) Speak(think string) (talk string) {
	if think == "sb" {
		talk = "你是个大帅比"
	} else {
		talk = "您好"
	}
	return talk
}

func main() {
    // this use the interface
	var peo People = &Student{}
	think := "bitch"
	fmt.Println(peo.Speak(think))
}

```

# 将nil指针转化为interface{}类型时会附加*main.TestStruct的信息
# 原因如下：_type代表了指针类型占了8个字节
type eface struct { // 16 字节
	_type *_type
	data  unsafe.Pointer
}

类型结构体 #
runtime._type 是 Go 语言类型的运行时表示。下面是运行时包中的结构体，其中包含了很多类型的元信息，例如：类型的大小、哈希、对齐以及种类等。

type _type struct {
	size       uintptr
	ptrdata    uintptr
	hash       uint32
	tflag      tflag
	align      uint8
	fieldAlign uint8
	kind       uint8
	equal      func(unsafe.Pointer, unsafe.Pointer) bool
	gcdata     *byte
	str        nameOff
	ptrToThis  typeOff
}
Go
size 字段存储了类型占用的内存空间，为内存空间的分配提供信息；
hash 字段能够帮助我们快速确定类型是否相等；
equal 字段用于判断当前类型的多个对象是否相等，该字段是为了减少 Go 语言二进制包大小从 typeAlg 结构体中迁移过来的4；
我们只需要对 runtime._type 结构体中的字段有一个大体的概念，不需要详细理解所有字段的作用和意义。

```go
package main

import "fmt"

type TestStruct struct{}

func NilOrNot(v interface{}) bool {
	return v == nil
}

func main() {
	var s *TestStruct
	fmt.Println(s == nil)    // #=> true
	fmt.Println(NilOrNot(s)) // #=> false
}
```
