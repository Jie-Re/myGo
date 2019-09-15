## GOPATH环境变量设置
GOPATH环境变量指定了工作空间位置。
1. 创建一个工作空间目录
    ```
    mkdir $HOME/work
    ```
    **注意：这个工作空间目录绝对不能和GO安装目录相同**
2. 设置相应的GOPATH
    ```
    export GOPATH=$HOME/work
    ```
    
    可通过`go env`检查配置结果：
    ![go env结果](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/goenv.PNG)
3. 作为约定，将此工作空间的`bin`子目录添加到环境变量PATH中：
    ```
    export PATH=$PATH:$GOPATH/bin
    ```

    可通过`echo $PATH`来检查配置结果：
    ![echo $PATH结果](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/echoPath.PNG)
## 设置包路径
标准库中的包有给定的短路径，比如 "fmt" 和 "net/http"。但对于**自己的包，必须选择一个基本路径**，来保证它不会与将来添加到标准库， 或其它扩展库中的包相冲突。
这里使用的是个人Github的账户作为基本路径：`github.com/Jie-Re`

在工作空间中创建一个目录，将源码放到其中：
```
mkdir -p $GOPATH/src/github.com/Jie-Re
```

## 第一个Go程序
1. 首先选择包路径（这里使用的是`github.com/Jie-Re/hello`），并在工作空间内创建相应的包目录：
    ```
    mkdir $GOPATH/src/github.com/Jie-Re/hello
    ```
2. 在该目录中创建名为`hello.go`的文件（使用VSCode）
    ```
    cd $GOPATH/src/github.com/Jie-Re/hello
    code hello.go
    ```
3. 编辑`hello.go`代码：
    ```
    package main

    import "fmt"
    
    func main() {
    	fmt.Printf("Hello, world.\n")
    }
    ```
    保存代码。接着根据VSCode的提示安装相应的包：点击`Install All`即可。
    ![editHelloGo](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/editHelloGo.PNG)  
    事实上，这一步往往会由于连接不上`golang.org`等问题而导致安装失败。  
    ![InstallFailed](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/InstallFailed.PNG)
    具体的错误解决办法可参加[我的博客](https://blog.csdn.net/xxiangyusb/article/details/100858000)  
3. 现在可以用go工具构建并安装此程序了：
    ```
    go install github.com/Jie-Re/hello
    ```
    P.S. 可以在系统的任何地方运行此命令。go 工具会根据`GOPATH`指定的工作空间，在`github.com/user/hello`包内查找源码。
    若从包目录中运行`go install`，也可以省略包路径：
    ```
    $ cd $GOPATH/src/github.com/user/hello
    $ go install
    ```
    此命令会构建`hello`命令，产生一个可执行的二进制文件。接着它会将该二进制文件作为`hello`安装到工作空间的`bin`目录中。在本文所述例子中为`$GOPATH/bin/hello`。  
    ![bin_hello](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/bin_hello.PNG)
4. 现在就可直接在命令行下输入`hello`的完整路径来执行它了：
    ```
    $GOPATH/bin/hello
    ```
    运行结果：  
    ![runResult](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/runResult.PNG)  
    事实上，由于我们前面已经将`$GOPATH/bin`添加到`PATH`中了，所以其实只需要输入该二进制文件名即可：  
    ```
    hello
    ```
    运行结果：  
    ![runResult2](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/runResult2.PNG)
5. 初始化仓库，添加文件并提交第一次更改到github仓库
    ```
    $ cd $GOPATH/src/github.com/Jie-Re/hello
    $ git init
    $ git add hello.go
    $ git commit -m "initial"
    ```
    接下来，通过以下命令将代码推送到远程仓库：
    ```
    $ git remote add origin git@github.com:Jie-Re/GoGo.git
    $ git push -u origin master
    ```
    推送代码到远程仓库过程中可能发生的错误解决办法以及进一步的操作可参见[我的博客](https://blog.csdn.net/xxiangyusb/article/details/100858000)

## 第一个Go库
下面编写一个库，并让`hello`程序来使用它
1. 选择包路径`github.com/Jie-Re/sringutil`并创建包目录
    ```
    mkdir $GOPATH/src/github.com/Jie-Re/stringutil
    ```
2. 在该目录中创建名为`reverse.go`的文件
    ```
    $ cd $GOPATH/src/github.com/Jie-Re/stringutil
    $ code reverse.go
    ```
3. 编辑`reverse.go`代码，保存，退出：
    ```
    // stringutil 包含有用于处理字符串的工具函数。
    package stringutil
    
    // Reverse 将其实参字符串以符文为单位左右反转。
    func Reverse(s string) string {
    	r := []rune(s)
    	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
    		r[i], r[j] = r[j], r[i]
    	}
    	return string(r)
    }
    ```
4. 用`go build`命令测试该包的编译：
    ```
    go build github.com/Jie-Re/stringutil
    ```
    若当前处在该包的根目录，则只需执行`go build`即可  
    这里不会产生输出文件。想要输出的话，必须使用`go install`命令，它会将包的对象放到工作空间的`pkg`目录中。
5. 确认`stringutil`包构建完毕后，修改原来的`hello.go`文件如下：
    ```
    package main
    
    import (
    	"fmt"
    
    	"github.com/Jie-Re/stringutil"
    )
    
    func main() {
    	fmt.Printf(stringutil.Reverse("!oG ,olleH"))
    }
    ```
6. 安装`hello`程序（此时`stringutil`包也会被自动安装）
    ```
    go install github.com/Jie-Re/hello
    ```
7. 运行`hello`
![helloGo](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/helloGo.PNG)
8. 查看工作空间目录树
    - 可通过`tree`来查看：
    `sudo yum install tree`（安装）
    - 安装后通过相关命令即可查看当前目录的目录树
    ```
    tree -a     # 显示所有
    tree -d     # 仅显示目录
    tree -L n   # n表示显示几层
    tree -f     # 显示完整路径
    ```
由于文件过多，故而以下只给出显示2层的结果：
![tree-L2](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/tree-L2.PNG)

## 测试
Go有一个轻量级的测试框架，它由`go test`命令和`testing`包构成。
1. 创建并编辑测试文件
    ```
    $ cd $GOPATH/src/github.com/Jie-Re/stringutil
    $ code reverse_test.go
    ```
    其内容如下：
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
    }
    ```
2. 使用`go test`运行该测试：
    ```
    go test github.com/Jie-Re/stringutil
    ```
    ![goTestResult](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/goTestResult.png)

## 远程包测试
```
$ go get github.com/golang/example/hello
$ $GOPATH/bin/hello
```
![remoteHello](https://github.com/Jie-Re/MyImages/raw/master/ServiceComputingGraphs/remoteHello.PNG)
