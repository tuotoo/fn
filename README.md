![Fn Project](http://fnproject.io/images/fn-300x125.png)

[![CircleCI](https://circleci.com/gh/fnproject/fn.svg?style=svg&circle-token=6a62ac329bc5b68b484157fbe88df7612ffd9ea0)](https://circleci.com/gh/fnproject/fn) [![GoDoc](https://godoc.org/github.com/fnproject/fn?status.svg)](https://godoc.org/github.com/fnproject/fn)
[![Go Report Card](https://goreportcard.com/badge/github.com/fnproject/fn)](https://goreportcard.com/report/github.com/fnproject/fn)

Fn 是一个事件驱动，开源，可在任何平台运行的 [FaaS](docs/serverless.md) 计算平台。
主要特性:

* 一次编写
  * [支持任何语言](docs/faq.md#which-languages-are-supported)
  * [支持 AWS Lambda 格式](docs/lambda/README.md)
* [跨平台](docs/faq.md#where-can-i-run-functions)
  * 公共私有混合云
  * [直接从 Lambda 导入函数](docs/lambda/import.md) and run them wherever you want
* [易于开发者使用](docs/README.md#for-developers)
* [便于运维管理](docs/README.md#for-operators)
* 用 [Go](https://golang.org) 编写
* 简单强大的扩展性


## 依赖

* Docker 17.05 或以上版本
* Docker Hub 账号 ([Docker Hub](https://hub.docker.com/)) (或其他 Docker 兼容的 registry)
* 登录进 Docker Hub 账号: `docker login`

## 快速开始

### 安装 CLI 工具

命令行工具不强制安装, 但是使用它会很方便. 安装方法:

#### 1. Homebrew - MacOS

如果你使用Mac并且使用 [Homebrew](https://brew.sh/) 的话, 可使用以下命令安装:

```sh
brew install fn
```

#### 2. Shell 脚本

在 Linux 和 MacOS 可以使用以下命令安装: 

```sh
curl -LSs https://raw.githubusercontent.com/fnproject/cli/master/install | sh
```

这条命令会下载一个 shell 脚本并执行. 因为脚本调用了 sudo , 所以有可能需要输入密码.

#### 3. 下载二进制文件

到 [releases](https://github.com/fnproject/cli/releases) 页面下载.

### 运行 Fn 服务器

启动 Fn 服务器:

```sh
fn start
```

这条命令会使用内置的数据库和消息队列, 以单服务器模式运行 Fn. 你可以在[这里](docs/operating/options.md)查看所有的配置项. 如果你在 Windows 下运行的话, 看[这里](docs/operating/windows.md).

### 编写你的第一个函数

函数是只完成一个简单任务的小巧强大的代码块. 在编写函数的时候, 不需要关注架构, 只需要关注你要函数处理的任务就可以了.

首先创建一个名为 `hello` 的空目录,然后 cd 进去.

下面是一个把字符串打印到 STDOUT (标准输出) 的简单的 Go 程序. 把代码复制粘贴到一个文件里面, 把文件命名为 `func.go` .

```go
package main

import (
  "fmt"
)

func main() {
  fmt.Println("Hello from Fn!")
}
```

现在运行下列 CLI 命令:

```sh
# 初始化函数
# 从上面的代码自动检测运行时, 并创建一个 func.yaml 文件
fn init

# 设置 Docker Hub 用户名
export FN_REGISTRY=<DOCKERHUB_USERNAME>

# 对函数进行测试
# 和在服务器上面一样, 函数会在一个容器中运行
fn run

# 把函数部署到 Fn 服务器上(默认为 localhost:8080)
# 这个命令还会为函数创建一个路由
fn deploy --app myapp
```

现在可以调用你的函数了:

```sh
curl http://localhost:8080/r/myapp/hello
# 或者:
fn call myapp /hello
```

或者在浏览器中: [http://localhost:8080/r/myapp/hello](http://localhost:8080/r/myapp/hello)

这样就完成了你的第一个函数的部署和调用. 更新你的函数只需要继续编写代码, 然后再次调用 `fn deploy myapp` 就可以了.

## UI

我们还开源了个 Web UI.只需要以下命令就可以运行:

```sh
docker run --rm -it --link functions:api -p 4000:4000 -e "API_URL=http://api:8080" fnproject/ui
```

访问 [https://github.com/fnproject/ui](https://github.com/fnproject/ui) 查看更多信息.


## 更多

* 查看我们的[函数系列教程](examples/tutorial/). 这个教程会通过一系列的示例演示一些 Fn 的核心功能. 我们会用大多数主流语言来编写示例代码, 是个很不错的起点教程.
* 查看我们的[完整文档](docs/README.md)
* 查看我们的[示例](/examples)
* 查看我们的[YouTube 频道](https://www.youtube.com/channel/UCo3fJqEGRx9PW_ODXk3b1nw)
* 查看我们的[API 文档](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/fnproject/fn/master/docs/swagger.yml)

## 获取帮助

* [在 StackOverflow 上面提问](https://stackoverflow.com/questions/tagged/fn) , 并打上 `fn` 的标签
* 加入 [Slack 社区](https://join.slack.com/t/fnproject/shared_invite/MjIwNzc5MTE4ODg3LTE1MDE0NTUyNTktYThmYmRjZDUwOQ)

## 参与进来

* 加入 [Slack 社区](http://slack.fnproject.io)
* 了解如何[贡献](CONTRIBUTING.md)
* 查看[里程碑](https://github.com/fnproject/fn/milestones)及相关的issues

## 及时获取信息

* [Blog](https://medium.com/fnproject)
* [Twitter](https://twitter.com/fnproj)
* [YouTube](https://www.youtube.com/channel/UCo3fJqEGRx9PW_ODXk3b1nw)
