# 测试函数

`fn` 内置了测试机制, 它可以创建一些输入和预期的输出, 以验证预期输出和实际输出.

## 编写测试文件

在函数目录中创建一个 `test.json` 文件 (和 `func.yaml` 文件同一级目录). 例如:

```json
{
    "tests": [
        {
            "input": {
                "body": {
                    "name": "Johnny"
                }
            },
            "output": {
                "body": {
                    "message": "Hello Johnny"
                }
            }
        },
        {
            "input": {
                "body": ""
            },
            "output": {
                "body": {
                    "message": "Hello World"
                }
            }
        }
    ]
}
```

上面的例子写了两个测试, 一个输入以下内容:

```json
{
    "name": "Johnny"
}
```

第二个测试没有输入内容.

第一个测试期望得到下面的 JSON 响应:

```json
{
    "message": "Hello Johnny"
}
```

第二个则会返回:

```json
{
    "message": "Hello World"
}
```

## 运行测试

直接在函数目录中运行:

```sh
fn test
```

你也可以在加上 `--remote` 参数, 一个远程的 `fn` 服务器上测试. 例如:

```sh
fn test --remote myapp
```

