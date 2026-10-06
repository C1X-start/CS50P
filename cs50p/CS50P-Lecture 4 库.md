
## 导入库的两种方式

1. 导入整个库

```python
import random
coin = random.choice(["heads","tails"])
print(coin)
```

- 导入 random 库中的所有函数
- 调用时必须指明库名：`库名.函数名()`
- `choice(seq)`：从序列（列表/元组）中随机抽取一个元素

2. 只导入指定函数

```python
from random import choice
coin = choice(["heads","tails"])
print(coin)
```

- 仅导入需要的函数，更轻量
- 调用时不需要再加库名前缀

## random 库常用函数

### randint(a, b)

```python
import random
number = random.randint(1,10)
print(number)
```

- 从 1 到 10 随机抽取整数，**左右边界都包含**

### shuffle 函数：打乱列表顺序（洗牌）

```python
import random
cards = ["a","b","c"]
random.shuffle(cards)
print(cards)
```

- 原地修改列表，**无返回值（返回 None）**
- ❌ 错误写法：`cards = random.shuffle([...])`，赋值后变量为 None

## statistics 库

### mean 函数：计算平均值

```python
import statistics
num = statistics.mean([1,1,3,4,1])
print(num)
```

## 命令行参数（command-line arguments）

无需程序运行后输入，在执行命令时直接传入参数。

### sys.argv（argument vector 参数向量）

```python
import sys
if len(sys.argv)<2:
    sys.exit("less")
elif len(sys.argv)>2:
    sys.exit("more")
print("hello",sys.argv[1])
```

- `sys.argv` 是一个列表，存放命令行输入的所有参数
- `sys.argv[0]` 是脚本文件名，第一个参数从 `sys.argv[1]` 开始
- `sys.exit("提示")`：异常退出程序，同时输出提示文字，避免输出长串报错

### 列表索引与切片

- `[1:]`：从第二个元素到最后
- `[1:-1]`：从第二个到倒数第一个（左闭右开，不包含最后一个）
- 负数索引：从列表末尾反向计数

## 第三方包（Package）

即第三方库，需要通过 `pip` 工具安装。

### 安装命令（终端执行）

```python
pip install cowsay
```

### 使用示例

```python
import cowsay
import sys
if len(sys.argv)==2:
    cowsay.cow("hello"+sys.argv[1])
```

- `cowsay.cow()` 只能接收一个字符串参数，多段内容用 `+` 拼接，不能用逗号分隔

## API 与 requests 库

`requests` 是第三方库，模拟浏览器发送网络请求，调用 API 接口。

### 基础示例

```python
import sys
import requests
import json
 
response = requests.get("[https://httpbin.org/get](https://httpbin.org/get)")
print(json.dumps(response.json(),indent=2))
```

### 关键知识点

1. **`response.json()`**
    - 是 `requests.get()` 返回的 **Response 对象的实例方法**，不是 requests 库的顶层函数
    - 作用：将接口返回的 JSON 字符串自动解析为 Python 字典/列表
    - 调用格式：`响应对象.json()`，无需写成 `response.requests.json()`
2. **`json.dumps(data, indent=2)`**
    - 内置 `json` 库的函数，将 Python 字典格式化为美观的 JSON 字符串
    - `indent=2`：设置缩进空格数，提升可读性
    - `json` 是 Python 内置库，无需额外下载
3. **params 传参（推荐写法）** 用字典传递查询参数，避免手动拼接 URL 字符串：
    
    ```python
    import requests
    params = {"name": "cs50p", "lesson": 4}
    response = requests.get("[https://httpbin.org/get](https://httpbin.org/get)", params=params)
    data = response.json()
    print(data["args"])
    ```
    
4. **状态码判断（最佳实践）** 调用 `.json()` 前先判断请求是否成功，避免接口异常时程序崩溃：
    
    ```python
    if response.status_code == 200:
        data = response.json()
    else:
        print("请求失败")
    ```
    

## 自定义模块（自己建造库）

将函数封装在 `.py` 文件中，其他脚本可以导入复用。

### 模块文件示例

```python
def main():
    hello("world")
    goodbye("world")

def hello(name):
    print(f"hello,{name}")

def goodbye(name):
    print(f"goodbye,{name}")

if __name__=="__main__":
    main()
```

- 不能直接在文件末尾调用 `main()`
- 必须放入 `if __name__=="__main__":` 条件中
- 作用：只有直接运行该文件时才执行 `main()`；其他文件导入该模块时，不会自动运行 `main()`，只导入函数

### 导入自定义模块

```python
from say import hello
import sys
if len(sys.argv)==2:
    hello(sys.argv[1])
```