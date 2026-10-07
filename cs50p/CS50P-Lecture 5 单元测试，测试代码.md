
## 一、import 模块查找规则与避坑

Python 导入模块的搜索优先级：

1. **当前工作目录下的同名 .py 文件**（优先级最高）
2. Python 内置标准库
3. 第三方库（site-packages 目录）

> ⚠️ 经典避坑：**永远不要用库名作为自己的 Python 文件名** 例如 `random.py`、`requests.py`、`cowsay.py`、`pytest.py`，会优先导入你自己的文件，覆盖真实的库，导致 `AttributeError: module 'xxx' has no attribute 'xxx'` 报错。

## 二、pytest 核心原则

pytest 测试的是**函数的返回值**，而非 `print()` 打印输出（print 属于程序副作用，pytest 无法捕获判断）。

### assert 断言关键字

- `assert 表达式`：断言表达式为真
- 表达式成立：程序正常继续，测试通过
- 表达式不成立：**抛出 AssertionError 异常**，测试失败（不是返回布尔值）

## 三、基础数值测试示例

测试 `squ.py` 中的 `square` 平方函数：

```Python
import pytest
from squ import square

def test_positive():
    assert square(2) == 4
    assert square(3) == 9
    assert square(4) == 16

def test_negative():
    assert square(-2) == 4
    assert square(-3) == 9

def test_zero():
    assert square(0) == 0
```

## 四、异常测试：pytest.raises

用于验证**函数在特定输入下是否抛出预期的异常**。

```python
def test_str():
    # 验证传入字符串时，square 函数会抛出 TypeError
    with pytest.raises(TypeError):
        square("cat")
```

- 写法：`with pytest.raises(异常类型):` 缩进写触发异常的代码
- 异常类型匹配则测试通过；不抛异常 / 抛其他异常则测试失败

## 五、多场景参数测试示例

测试 `hello.py` 中的 `hello` 函数：

```python
from hello import hello

def test_default():
    # 测试默认参数
    assert hello() == "hello,world"

def test_arguments():
    # 循环测试多组输入
    for name in ["ron", "ken", "li"]:
        assert hello(name) == f"hello,{name}"
```

## 六、pytest 运行规则与命名规范

### 1. 文件放置

**测试文件和被测试的源文件必须放在同一个文件夹**。

### 2. 命名规则（pytest 自动识别）

- **测试文件**：文件名以 `test_` 开头，或以 `_test.py` 结尾
- **测试函数**：函数名必须以 `test_` 开头，否则 pytest 不会自动识别执行

### 3. 运行命令（CS50P 推荐写法）

终端切换到对应文件夹，执行：

```
python -m pytest
```

会自动扫描当前目录下所有符合命名规则的测试文件，执行全部 test_ 开头的函数。

> ✅ 为什么推荐 `python -m pytest` 而非直接 `pytest`
> 
> - 直接 `pytest`：调用 PATH 里的 pytest.exe，其绑定的 Python 环境可能和你代码运行的环境不一致，容易出现 `ModuleNotFoundError`
> - `python -m pytest`：使用**当前终端的 Python 解释器**加载 pytest，模块搜索路径和你平时写代码完全一致，规避环境不一致导致的导入失败

## 七、常见报错与解决方案

### 1. ModuleNotFoundError: No module named 'xxx'

- 原因1：测试文件和源文件不在同一个文件夹 → 放到同一目录
- 原因2：终端工作目录不对 → `cd` 切换到文件所在文件夹
- 原因3：直接 `pytest` 环境不一致 → 改用 `python -m pytest`
- 原因4：文件名和库名重名 → 重命名自己的 py 文件

### 2. collected 0 items（收集到 0 个测试）

- 测试函数没有以 `test_` 开头 → 重命名函数
- 测试文件不符合命名规则 → 改为 `test_xxx.py` 或 `xxx_test.py`