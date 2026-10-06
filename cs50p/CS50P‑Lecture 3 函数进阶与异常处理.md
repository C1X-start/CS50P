
tags: ["#CS50P", "#Python", "#Lecture3", "#异常处理"]
---

# CS50P｜Lecture 3 异常处理（精简入门版）
> 解决的问题：用户输入非法内容时，程序不会直接崩溃，能友好提示并继续运行

---

## 1. 核心语法：try - except
### 作用
- `try`：放**可能会出错**的代码
- `except 错误类型`：出错后执行这里的代码，程序不会中断

### 最简示例
```python
try:
    x = int(input("请输入数字："))
except ValueError:
    print("输入错误，请输入整数！")
````

✅ 说明：

- `ValueError`：专门捕获「值类型不匹配」错误，比如把字母转成整数
- 错误类型**首字母必须大写**，写错了捕获失效

---

## 2. else 子句：成功才执行

### 规则

`else` 里的代码，**只有 try 中代码完全没出错时，才会运行**。

### 新手必踩坑

```python
try:
    x = int(input("请输入数字："))
except ValueError:
    print("输入错误")

print(f"你输入的是{x}")  # ❌ 输入错误时x根本没创建，直接报NameError
```

### 正确写法

```python
try:
    x = int(input("请输入数字："))
except ValueError:
    print("输入错误")
else:
    print(f"你输入的是{x}")  # ✅ 只有赋值成功，才会走到这
```

---

## 3. pass：静默处理错误

明知可能出错，但不需要提示、不需要处理时，用 `pass` 占位：

```python
try:
    x = int(input())
except ValueError:
    pass  # 出错了什么也不做，程序继续往下走
```

---

## 4. 最常用场景：循环校验输入

搭配 `while True` 无限循环，直到用户输入正确才退出：

```python
while True:
    try:
        x = int(input("请输入整数："))
    except ValueError:
        print("输入无效，请重新输入！")
    else:
        print(f"输入成功，x = {x}")
        break  # 输入正确，跳出循环
```

---

## ⚠️ 易错点清单

1. **错误类型大小写错**：`valueerror` 不行，必须是 `ValueError`
2. **变量作用域坑**：try 里定义的变量，不要拿到外面直接用，放 `else` 里最安全
3. **只有 try 没有 except**：语法错误，必须配对出现
4. **except 后不写错误类型**：会捕获所有错误，不推荐新手用，容易隐藏真实问题