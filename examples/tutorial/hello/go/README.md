# Tutorial 1: Go Function w/ Input (3 minutes)

这个例子将告诉你如何测试和部署 Go 代码到 Fn. 它还会演示如何通过 stdin 传输数据.

### 首先, 运行下列命令:

```sh
# 初始化函数, 并创建 func.yaml 文件
fn init --name hello-go

# 测试函数的效果. 会在一个函数中运行, 就像在服务器中所表现的一样
fn run

# 现在试试用 stdin 输入下数据
cat sample.payload.json | fn run

# 把函数部署到 Fn 服务器上 (默认是 localhost:8080)
# 同时也会创建一个到该函数的路由
fn deploy --app myapp
```

### 现在调用上面的函数:

```sh
curl http://localhost:8080/r/myapp/hello-go
```

或者在浏览器打开: [http://localhost:8080/r/myapp/go](http://localhost:8080/r/myapp/hello-go)

再试试输入 JSON 数据:

```sh
curl -H "Content-Type: application/json" -X POST -d @sample.payload.json http://localhost:8080/r/myapp/hello-go
```

That's it!

### 关于依赖

对于 Go 程序, 直接把依赖放到 `vendor/` 目录就可以了.

# In Review

1. 我们在命令行通过管道的方式将 JSON 数据传给函数
    ```sh
    cat sample.payload.json | fn run
    ```

2. 我们在函数中通过 **stdin** 接收输入
    ```go
    json.NewDecoder(os.Stdin).Decode(p)
    ```

3. 我们将输出写到 **stdout**
    ```go
    fmt.Printf("Hello")
    ```

4. 我们将 **stderr** 发送到系统日志
    ```go
    log.Println("here")
    ```


# 接下来
## [第二部分: 输入参数](../../params)
