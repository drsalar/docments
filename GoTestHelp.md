# GO test
**示例项目结构**

>hello
    hello.go
    hello_test.go
    hello_bench_test.go
    hello_cpu_test.go
    
**hello.go:**

```
package main

import (
	"fmt"
)

func main() {
	SayHello()
}

func hello() {
	fmt.Println("Hello World!")
}
```

## 功能测试
**示例代码，hello_test.go:**
```
package main

import (
	"testing"
)

func Test_SayHello(t *testing.T) {
	SayHello()
}
```
**说明**
* 测试文件必须以_test结尾
* 测试文件的package必须与被测试文件一致
* 导入testing包
* 测试函数必须以Test开头，例如TestXxxx或者TestXxxx
* 参数必须为(t *testing.T)

**测试指令：**
1. 测试所有文件
`go test`
2. 显示测试详细信息
`go test -v`
3. 显示测试覆盖率
`go test -cover`

**测试结果说明**
```
Hello World!
PASS
coverage: 50.0% of statements
ok  	hello	0.001s
```
PASS:测试通过
FAILED:测试未通过
coverage: 50.0% of statements　测试代码的覆盖率为５０％

## 性能测试
**示例代码，hello_bench_test.go:**
```
package main

import (
	"testing"
)

func BenchmarkSayHellp(b *testing.B) {
	for i := 0; i < b.N; i++ {
		SayHello()
	}
}
```
**说明**
*  测试文件必须以_test结尾
*  测试文件的package必须与被测试文件一致
*  导入testing包
*  测试函数必须以Benchmark开头，例如Benchmark_Xxxx或者BenchmarkXxxx
*  参数必须为(b *testing.B)

**测试指令：**
1. 测试所有的方法
`go test -bench='.'`
2. 测试制定的方法
`go test -bench='YourTestFunc'`

**测试结果说明：**
`BenchmarkSayHellp-4    1000000     2113ns/op`
测试方法名－ｃｐｕ个数　循环次数　每次循环耗时

## ＣＰＵ使用情况分析报告
**示例代码，hello_cpu_test:**
```
package main

import (
	"fmt"
	"os"
	"runtime/pprof"
	"testing"
)

func BenchmarkSayHellp(b *testing.B) {
	f, err := os.Create("cpu.prof")
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	pprof.StartCPUProfile(f)
	defer pprof.StopCPUProfile()
	for i := 0; i < b.N; i++ {
		SayHello()
	}
}
```
**说明：**
* 导入os, runtime/pprof包
* os.Create("cpu.prof")中的参数为生成的cpu适用情况分析文件名
* 在需要测试cpu使用情况的代码前开启pprof: pprof.StartCPUProfile(f)
* 在cpu测试结束的地方停止pprof:pprof.StopCPUProfile()

**指令：**
1. 运行测试文件，生成cpu.prof
2. 使用pprof，进入交互界面
`go tool pprof hello.test cpu.prof`
其中hello.test为cpu.prof对应的二进制文件
３． 输入help查看命令

**生成状态分析图：**
下载graphviz，若无法安装需要下载libgraphviz4等依赖包，部分包在ftp://192.168.96.62/incoming/常用软件/　目录下,若需要其他依赖包，请访问官网[](http://www.graphviz.org/Download.php)下载
安装完成后在pprof交互界面中输入web命令即可生成svg图
