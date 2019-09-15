# 安装golang
- 采用终端命令来安装golang

	```
	$ sudo yum install golang
	```
- 可以查看安装的版本和安装目录
	

	```
	$ rpm -ql golang |more
	```
	```
	$ go version
	```
可以看到golang语言已经安装在了如下目录，版本是11.5
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915100637567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915101350137.png)
# 配置环境变量
1. 创建工作空间
	

	```
	$ mkdir $HOME/gowork
	```
2. 在 ~/.profile 文件中添加

	```
	export GOPATH=$HOME/gowork
	export PATH=$PATH:$GOPATH/bin
	```
	然后执行`$ source $HOME/.profile` 令配置的环境生效
3. `go env` 检查配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091510403127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
GOPATH和GOROOT均添加成功
# 运行Hello world
- 安装并配置成功后，我们就可以试着编写一个go语言小程序了
 1. 可以在终端输入`$ go` 了解一系列go命令的执行方法：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091510454783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
 2. 在工作空间内创建相应的包目录
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915124147526.png)
 4. 在该目录下创建hello.go文件，编写一个输出“hello world"的程序：
 

	```
	package main

	import "fmt"

	func main() {
  	  fmt.Printf("hello, world\n")
	}
	```
3. 用go工具构建并安装此程序

	```
	$ go install github.com/user/hello
	```

4. 这样，我们可以在终端直接运行生成的二进制文件或者运行`go run hello.go`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915125715756.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915105037150.png)
5. 然后，我们可以初始化仓库，并提交第一次文件

	```
	$ cd $GOPATH/src/github.com/user/hello
	$ git init
	$ git add hello.go
	$ git commit -m "initial commit"
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915134625944.png)
## 建立库和测试
- 编写一个库，让hello程序使用它
- 在如下路径创建包目录：
	

	```
	$ mkdir $GOPATH/src/github.com/user/stringutil
	```
	在该目录中创建reverse.go文件，内容如下：
	

	```
	package stringutil

	func Reverse(s string) string {
		r := []rune(s)
		for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
			r[i], r[j] = r[j], r[i]
		}
		return string(r)
	}
	```
	可以用`$ go build` 来测试该程序
- 修改hello.go文件，在文件中调用stringutil包：

	```
	package main

	import (
		"fmt"

		"github.com/user/stringutil"
	)

	func main() {
		fmt.Printf(stringutil.Reverse("!oG ,olleH"))
	}
	```
	然后，我们执行hello文件时，就能看到一条反向字符串的输出：
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915143457122.png)
	- go可以从远程代码库自动获取包，即用`$ go get`指令实现远程导入
	- $`go get` 可以从Mercurial，Git，Subversion，Bazaar等系统检查出代码包
	- go有一个轻量级的测试框架，由`$ go test` 命令和testing包构成
	我们可以为stringutil编写一个测试文件，内容如下：
	

	```
	package stringutil

	import "testing"

	func TestReverse(t *testing.T) {
		cases := []struct {
			in, want string
		}{
			{"Hello, world", "dlrow ,olleH"},
			{"Hello, 世界", "界世 ,olleH"},
			{"", ""},
		}
		for _, c := range cases {
			got := Reverse(c.in)
			if got != c.want {
				t.Errorf("Reverse(%q) == %q, want %q", c.in, got, c.want)
			}
		}
	```
- go test 运行该测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091519331734.png)
# 安装必要的工具和插件
## 安装git客户端
- 使用如下指令安装git客户端：

	```
	sudo yum install git
	```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915115843719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
可以看到我们已经安装了当前最新版本的git客户端

## 安装VSCode编辑器
为了方便在CentOS中进行编程，我们可以安装VSCode这一轻量级的编辑器。

```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" 
```
用yum指令安装

```
yum check-update
sudo yum install code
```
安装成功后，打开界面如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915114604105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
## 安装一些go工具
进入VSCode首页后，会提示我们要安装一些go语言的工具
但是由于https://golang.org/被wall，我们不能通过官方方法安装...
所以我们在本地创建一个目录，然后去github上下载：

```
mkdir $GOPATH/src/golang.org/x/
go get -d github.com/golang/tools
```
发现无法下载源码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915205003386.png)
我们可以进入目录后clone下来：

```
cd $GOPATH/src/golang.org/x
git clone https://github.com/golang/tools.git
```
最后进行安装

```
go install golang.org/x/tools/cmd/goimports
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915205836638.png)
# 更多有关golang
## 安装gotour
go tour是Google搭建的基于浏览器的交互式go编程指南，可以有效的帮助我们学习go语言
使用如下指令安装并运行go tour

```
	$ go get github.com/Go-zh/tour/gotour
	$ gotour
```

## go语言编程练习
- 到这里，我的go语言开发环境就已经安装得差不多了。
现在可以利用已有的工具和学习资源完成一些编程练习。
比如，用go语言实现一个快速排序算法：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915121205829.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrYW5pbmU=,size_16,color_FFFFFF,t_70)
程序链接： http://139.9.57.167:20080/share/blv3b5ed0lit3phfpi50?secret=false
