# 详细使用

这篇文档介绍了开发者常用的一些命令.

### 创建应用

应用本质上就是一组函数放在一起, 共同组成了 API . 使用下列命令创建应用:

```sh
fn apps create myapp
```

或者使用 cURL:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "app": { "name":"myapp" }
}' http://localhost:8080/v1/apps
```

[More on apps](docs/apps.md).

创建好应用之后, 就可以通过路由访问函数了.

### 添加路由

路由是在应用中所定义的路径到函数的映射. 在下面的示例中, 我们会将 `/hello` 映射到一个我们之前上传的 `fnproject/hello` 的 `Hello World!` 函数上 -- 是的, 你可以把写好的函数共享出来. 这个函数的代码在[examples directory](examples/hello/go).
点击[编写函数](docs/writing.md)查看更多.

```sh
fn routes create myapp /hello -i fnproject/hello
```

或者使用 cURL:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "route": {
        "path":"/hello",
        "image":"fnproject/hello"
    }
}' http://localhost:8080/v1/apps/myapp/routes
```

[关于路由](docs/routes.md).

### 调用函数

调用函数就跟请求 URL 一样简单. 每个应用都有自己的命名空间, 每个路由也都映射到了应用上.
比如, 我们上边在应用 `myapp` 中创建的 `/hello` 路由就可以通过这个 URL 调用:
 http://localhost:8080/r/myapp/hello

可以用浏览器直接访问或者使用 `fn` 来调用:

```sh
fn call myapp /hello
```

或者通过 cURL 调用:

```sh
curl http://localhost:8080/r/myapp/hello
```

### 向函数传递数据

函数可以在 STDIN 中获取 HTTP 请求的 body, 而请求的头部信息则在环境变量里面, 你可以使用命令行工具对函数进行测试: 

```sh
echo '{"name":"Johnny"}' | fn call myapp /hello
```

或者使用 cURL:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "name":"Johnny"
}' http://localhost:8080/r/myapp/hello
```

现在返回的信息就应该是 `Hello Johnny!` 而不是 `Hello World!` 了.

### 添加异步函数

我们在上面所使用的是 FN 的同步函数, 而 Fn 也支持异步函数用于后台处理.

[异步函数](async.md) 用于比较耗 CPU 或者耗时比较长的任务处理.
比如: 图片处理, 视频处理, 数据处理, ETL 等.
架构上, 同步和异步的主要区别是, 异步函数会放在一个队列里面, 当资源可用的时候才去执行, 这样的话就不会对需要快速响应的同步 API 造成干扰.
另外, 由于异步函数使用了消息队列, 所以你可以在队列里面放上百万个函数调用而不用担心容量问题.

只要添加一个 `"type":"async"` 的路由就完成异步路由的添加了, 例如:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "route": {
        "type": "async",
        "path":"/hello-async",
        "image":"fnproject/hello"
    }
}' http://localhost:8080/v1/apps/myapp/routes
```

或者在 `func.yaml` 中设置 `type: async` .

现在你请求这个路由的时候:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "name":"Johnny"
}' http://localhost:8080/r/myapp/hello-async
```

在返回结果会得到一个 `call_id` :

```json
{"call_id":"572415fd-e26e-542b-846f-f1f5870034f2"}
```

如果你查看日志, 你会看到函数实际上在后台运行了:

![async log](docs/assets/async-log.png)

查看更多关于 [日志](docs/logging.md) 的内容.