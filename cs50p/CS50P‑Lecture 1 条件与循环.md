
tags: ["#CS50P", "#Python", "#Lecture1", "#条件判断"]
source: CS50P Lecture 1 条件与循环
created: 2026-10-04
---

# CS50P｜Lecture 1 条件判断与布尔逻辑

> 🔗上一节：[[CS50P-Lecture 0 变量、字符串与函数基础]]
> 🔗下一节：[[CS50P‑Lecture 2 循环与迭代]]

## 📌本节总览
- 掌握 `if / elif / else` 条件分支结构与语法规范
- 理解布尔（bool）类型本质，掌握 `and` / `or` / `not` 逻辑运算符
- 学习三种布尔返回值的写法，体会Python的简洁表达风格
- 掌握 `match` 模式匹配语句，对比多分支if的差异与适用场景
- 梳理条件判断的高频易错点，规避新手常见语法错误

## 📖核心知识点

### 一、if 条件分支基础
#### 1. 基础语法结构
```python
if 条件表达式:
    满足条件执行的代码
elif 条件表达式:
    满足该条件执行的代码
else:
    以上都不满足时执行的代码
````

#### 2. 语法核心规则

1. 所有分支行尾必须加**冒号 `:`**
2. 分支内部的代码必须**统一缩进**（标准为4个空格）
3. 条件表达式外层**不需要括号**（区别于C/Java等语言）

#### 3. 逻辑运算符

用于组合多个判断条件：

|运算符|含义|规则|
|---|---|---|
|`and`|逻辑与|两边同时为 True，结果才为 True|
|`or`|逻辑或|任意一边为 True，结果就为 True|
|`not`|逻辑非|对布尔值取反|

示例：

```
if x == "Harry" or x == "Ron" or x == "Hemin":
    print("Granfenduo")
```

---

### 二、布尔类型（bool）

- 只有两个固定取值：`True`（真）、`False`（假）
- `if` 语句的核心本质：判断条件表达式最终的布尔结果
- 所有比较运算（`==`、`!=`、`>`、`<`、`>=`、`<=`）的返回值本身就是布尔类型

#### 三种布尔返回写法（以判断偶数为例）

写法逐步精简，是Python简洁性的典型体现：

```
# 写法1：完整 if-else 分支返回
def parity(n):
    if n % 2 == 0:
        return True
    else:
        return False

# 写法2：三元表达式精简
def parity_2(n):
    return True if n % 2 == 0 else False

# 写法3：直接返回表达式结果（最推荐）
def parity_3(n):
    return n % 2 == 0
```

> 原理：`n % 2 == 0` 这个运算的结果本身就是布尔值，无需额外的 if 判断包装。

完整调用示例：

```
def main():
    x = int(input("what's is x?"))
    if parity_3(x):
        print("even")
    else:
        print("odd")

main()
```

---

### 三、match 模式匹配语句

功能类似C语言的 `switch`，用于**固定值多分支精准匹配**，相比多层 `if-elif` 结构更清晰、匹配效率更高。

#### 基础语法

```
def house_2(x):
    match x:
        case "Harry":
            print("Granfenduo")
        case "Ron":
            print("Granfenduo")
        case "Hemin":
            print("Granfenduo")
        case _:
            print("who")
```

#### 语法注意事项

1. `match` 和每一行 `case` 末尾都必须加冒号
2. case 分支内的代码需要缩进
3. **不需要写 `break`**，匹配成功后自动结束匹配
4. `case _:` 代表默认情况，对应其他语言的 `default`
5. case 后匹配字符串时，必须加引号，否则会被识别为变量

#### 优化写法：多值合并匹配

多个 case 执行相同代码时，可以用 `|` 合并：

```
match x:
    case "Harry" | "Ron" | "Hemin":
        print("Granfenduo")
    case _:
        print("who")
```

#### if-elif 与 match 对比

|写法|适用场景|特点|
|---|---|---|
|`if-elif`|范围判断、复杂逻辑条件|灵活度高，支持任意表达式|
|`match`|固定值精准匹配|结构清晰，可读性强，代码更整洁|

## ❗易错点 & 踩坑记录

1. ⚠️ `if` / `elif` / `else` / `match` / `case` 行尾漏写冒号，是新手最高频语法错误
2. ⚠️ 分支内代码忘记缩进，或缩进不一致，会触发 `IndentationError`
3. ⚠️ 不要把 `elif` 写成 `else if`，Python 没有这种写法
4. ⚠️ `match` 的 `case` 匹配字符串时忘记加引号，会被识别成变量导致报错
5. ⚠️ 布尔值首字母必须大写：`True` / `False`，小写 `true` 会直接报错
6. ⚠️ 条件判断相等用 `==`，不要误写成赋值符 `=`
7. ⚠️ `case _` 是下划线，不要写成减号或其他符号

## 💻常用代码汇总

```
# 1. 最简奇偶判断
def is_even(n):
    return n % 2 == 0

# 2. 标准多分支条件
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 60:
        return "C"
    else:
        return "D"

# 3. match 多值匹配
def house_match(name):
    match name:
        case "Harry" | "Ron" | "Hemin":
            return "Granfenduo"
        case "Dracon":
            return "Silaitelin"
        case _:
            return "who?"

# 标准主函数结构
def main():
    x = int(input("what's is x?"))
    print("even" if is_even(x) else "odd")

if __name__ == "__main__":
    main()
```

## 📎关联概念跳转

- [[CS50P-Lecture 0 变量、字符串与函数基础]]
- [[CS50P‑Lecture 2 循环与迭代]]
- [[Python 浮点数精度深度解析]]