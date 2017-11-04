# 编写函数

这篇文档会向你介绍如何编写函数. 你也可以使用更高级的抽象比如 [lambda](lambda/README.md).

在[示例目录](/examples)有几种语言编写的示例.

## 代码编写

大多数语言写出来的伪代码结构就像代码这样:

```ruby
# Read and parse from STDIN
body = JSON.parse(STDIN)

# Do something
return_struct = doSomething(body)

# If sync, respond:
STDOUT.write(JSON.generate(return_struct))
# If async, update something:
db.update(return_struct)
```

## 输入

输入有标准输入和环境变量提供. 在这里, 我们只讨论默认的输入和环境变量, 其他情况请参照[这里](function-format.md).
直接读取 STDIN, 就可以获取到函数的请求, 其他的变量可以在以下环境变量获取:

* `FN_REQUEST_URL` - 请求的完整 URL 路径 ([解析示例](https://github.com/fnproject/fn/tree/master/examples/tutorial/params))
* `FN_APP_NAME` - 路由中的应用名称, 如: `myapp`
* `FN_PATH` - 命中的路由, 如: `/hello`
* `FN_METHOD` - 请求所用的 HTTP 方法名, 如: `GET` 或者 `POST`
* `FN_CALL_ID` - 每个函数执行的唯一ID
* `FN_FORMAT` - [函数格式](function-format.md)字符串, 现在只有 `default` 和 `http`. 默认是 `default`.
* `FN_MEMORY` - 函数可以分配的内存大小, 以 MB 为单位
* `FN_TYPE` - 函数的调用类型 'sync' 或者 'async'
* `FN_HEADER_$X` - 请求所使用的 HTTP 头部, 将 $X 替换为全部大写的头部名称, 并将头部名称中的中划线改为下划线.
  * `$X` - any [configuration values](https://github.com/fnproject/cli/blob/master/README.md#application-level-configuration) you've set
  for the Application or the Route. Replace X with the upper cased name of the config variable you set. Ex: `minio_secret=secret` will be exposed via MINIO_SECRET env var.
* `FN_PARAM_$Y` - 通过URL传递的参数. 将 $Y 替换为 URL 中类似 `:var` 的参数.

Warning: these may change before release.

## 日志

在同步函数中, 标准输出会作为响应数据. 所以应该把日志写到标准错误输出中, 点击查看[这样做的目的](http://www.jstorimer.com/blogs/workingwithcode/7766119-when-to-use-stderr-instead-of-stdout).

所以要输出日志的话, 只要把日志写到 STDERR 就可以了. 以下是集中语言的示例.

在 Go 语言中, 直接使用 [log](https://golang.org/pkg/log/) 库就可以了, 它默认就是写到标准错误输出的.

```go
log.Println("hi")
```

在 Node.js:

```node
console.error("hi");
```

[More details for Node.js here](http://stackoverflow.com/a/27576486/105562).

在 Ruby:

```ruby
STDERR.puts("hi")
```

## 使用 Lambda 函数

### Lambda everywhere

Fn 的 Lambda 支持使你可以在任何平台运行 AWS Lambda 函数, 你可以不改动代码而在 Fn 上运行.

创建 Lambda 函数和创建普通函数是一样的, 只要加上 `lambda-node` 运行时参数就可以了.

```sh
fn init --runtime lambda-node --name lambda-node
```

需要注意的是, 你的主函数必须是 `func.js`.

TODO: Make Java and Python use the new workflow too.

## 下一步

* [打包函数](packaging.md)
