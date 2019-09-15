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
    具体的错误解决办法可参加[我的博客]()
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

