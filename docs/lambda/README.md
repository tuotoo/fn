# Lambda everywhere

Fn 的 Lambda 支持使得你可以在任何地方运行 AWS Lambda 函数.你可以无需任何修改的运行你的代码.

## 创建 Lambda 函数

创建 Lambda 函数和创建普通函数一样, 只要加上 `lambda-node-4` 运行时就可以了.

```sh
fn init --runtime lambda-node-4 --name lambda-node
```

你的主文件必须是 `func.js`

TODO: Make Java and Python use the new workflow too.
