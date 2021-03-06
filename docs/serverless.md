# 什么是 Serverless/FaaS?

Serverless 是一种可以为开发者和运维人员提供简单、高效、可扩展的新型架构范式. 区别开这两个角色是很重要的， 因为这种架构对于他们的好处是不同的：

## 对于开发者

对大多数人来说, 主要面向开发者的好处有::

* 不需要管理服务器(serverless), 只需要上传代码就可以了, serverless会处理底层架构相关的东西
* 编码超级简单, 只需要少量的代码便可以实现需要的功能
* 启动执行代码的成本是毫秒级的, 不需要全天候 24/7 小时运行, 每个功能都只在需要的时候才运行, 极大降低成本

因为你只在本地运行 Fn, 你可能注意不到节省下来的在基础设置上运维成本, 那么请你往下看

## 对于运维

如果你负责Fn的运维(负责serverless服务的服务器的维护), 那么好处是不一样的, 但也是相关联的:

* 极大减少资源占用
  * 不像 app/API/微服务 那样不管有没有在使用都需要 24/7 小时的占用资源, 服务在基础设施上只有运行的时候才会占用资源
* 方便管理和扩展
  * 可以用任何语言或者任何技术编写代码去维护单个系统
  * 只需要监控单个系统
  * 对于整个系统的扩容, 不需要单独扩容每个应用
  * 扩容只需要简单的添加更多的 Fn 节点

只要搜索
["什么是 serverless"](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=what%20is%20serverless)
便可以获得相关的更多信息.
