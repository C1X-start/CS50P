
tags: ["#CS50P", "#Python", "#Lecture2", "#循环", "#列表", "#字典"]
source: CS50P Lecture 2 循环与迭代
created: 2026-10-05
---

# CS50P｜Lecture 2 循环、列表与字典

> 🔗上一节：[[CS50P｜Lecture 1 条件判断与布尔逻辑]]
> 🔗下一节：[[CS50P‑Lecture 3 函数进阶与异常处理]]

## 📌本节总览
- 掌握 `while` 与 `for` 两种循环结构的语法与适用场景
- 理解 `range()` 函数的左闭右开特性，掌握下划线 `_` 计数惯例
- 学习第五种数据类型：**列表 list**，掌握索引、长度获取与遍历方法
- 学习第六种数据类型：**字典 dict**，理解键值对结构与遍历方式
- 理解变量作用域，掌握 `while True` 无限循环 + `return` 退出的写法
- 了解列表嵌套字典的结构化数据表达与遍历方法

## 📖核心知识点

### 一、循环结构
#### 1. while 循环
满足条件就重复执行代码块，语法格式和 `if` 一致。
```python
i = 0
while i < 3:
    print("meow")
    i += 1
````

⚠️ 核心注意点：

- 循环体内必须包含**让条件趋于不成立**的逻辑，否则会变成死循环
- Python 中没有 `i++` / `i--` 写法，自增写为 `i += 1`，自减写为 `i -= 1`
- 行尾必须加冒号，内部代码统一缩进

#### 2. print 快捷重复写法

字符串可以用 `*` 直接重复多次，可替代简单的计数循环：

```
print("meow\n" * 3)
```

#### 3. for 循环 + range()

用于**固定次数**的循环，搭配 `range()` 生成整数序列。

```
for i in range(3):
    print("meow")
```

- `range(3)` 生成 `0, 1, 2` 三个整数，**左闭右开**，不包含结束值 3
- 循环变量 `i` 从 0 开始，依次取序列中的值

##### 下划线命名惯例

当循环变量仅用作计数、不会在循环体内使用时，约定用 `_` 命名，表示“无用变量”：

```
for _ in range(3):
    print("meow")
```

#### 4. while True 无限循环

`while True` 会创建永久执行的循环，必须通过 `return` 或 `break` 退出。 常用来做输入校验，直到获取合法值才返回：

```
def get_positive_number():
    while True:
        x = int(input("what's x?"))
        if x > 0:
            return x  # return 会直接跳出循环并结束函数
```

#### 5. 变量作用域

- 函数内部定义的变量是**局部变量**，仅在函数内部生效，和外界无关
- 不同函数之间的变量互不干扰

##### 完整示例：循环 + 函数拆分

```
def main():
    meow(get_number())

def meow(n):
    for _ in range(n):
        print("meow")

def get_number():
    while True:
        x = int(input("what's x?"))
        if x > 0:
            return x

main()
```

---

### 二、列表 list（第五种数据类型）

一组有序元素的集合，用**方括号 `[]`** 表示，元素之间用逗号分隔。

#### 基础特性

- 有序存储：通过**位置索引**访问元素，索引从 `0` 开始
- `len(列表)`：返回列表的元素个数（长度）
- `列表[索引]`：获取对应位置的元素

```
students = ["Harry", "Ron", "Hemin"]
print(students[0])   # 第一个元素 Harry
print(len(students)) # 长度 3
```

#### 两种遍历方式

|遍历方式|写法|适用场景|
|---|---|---|
|直接遍历元素|`for 元素 in 列表:`|只需要元素值，不需要索引|
|按索引遍历|`for i in range(len(列表)):`|需要同时用到序号和元素|

示例：按索引遍历并输出序号

```
students = ["Harry", "Ron", "Hemin"]
for i in range(len(students)):
    print(i + 1, students[i])
```

##### 常用内置函数与方法
- `列表.append(元素)`：在列表末尾添加一个元素
- `max(列表)` / `min(列表)`：返回列表中的最大/最小值
- `sum(列表)`：返回列表所有元素的和
- `len(列表)`：返回列表长度（元素个数）
-  `pop()` 方法从队列的开头删除一个元素并返回它


### 三、字典 dict（第六种数据类型）

**键值对（key-value）**结构，用来存储事物的对应关系，类似字典的“单词-释义”。用**花括号 `{}`** 表示。

#### 基础语法

```
house_dict = {
    "Harry": "Gran",
    "Ron": "Gran",
    "Hemin": "Gran",
    "Draco": "Silai"
}
```

- 键（key）和值（value）用冒号 `:` 分隔
- 键值对之间用逗号分隔
- 通过 `字典[键]` 的方式获取对应的值
##### 常用操作
- `键 in 字典`：判断键是否存在于字典中，返回布尔值，是字典查询的标准写法
- `字典.items()`：返回所有键值对，用于遍历同时获取键和值
```python
for key, value in dict.items():
    print(key, value)
````

#### 遍历字典

for 循环遍历字典时，**默认遍历的是键（key）**：

```
for student in house_dict:
    print(student, house_dict[student], sep=",")
```

#### 列表嵌套字典

把多个字典放进列表中，每个字典代表一条完整数据，适合表达结构化信息：

```
students = [
    {"name": "1a", "house": "A", "pet": "aaa"},
    {"name": "1b", "house": "B", "pet": "bbb"},
    {"name": "1c", "house": "C", "pet": "ccc"},
    {"name": "1d", "house": "D", "pet": "ddd"}
]

for student in students:
    print(student["name"], student["house"], student["pet"])
```

遍历逻辑：先依次取出列表中的每个字典，再通过键取出对应的值。

## ❗易错点 & 踩坑记录

1. ⚠️ Python 没有 `i++` 自增语法，必须写 `i += 1`
2. ⚠️ `range(n)` 范围是 0 到 n-1，不包含 n，循环次数容易多算一次
3. ⚠️ 列表索引从 0 开始，第一个元素是 `list[0]` 不是 `list[1]`
4. ⚠️ `while True` 必须设置退出条件（`return` / `break`），否则会无限卡死
5. ⚠️ 字典取值必须用**方括号** `dict[key]`，不要写成圆括号
6. ⚠️ 字典的键是唯一的，重复的键会覆盖之前的值
7. ⚠️ 循环体、条件体忘记缩进，或缩进不一致会触发语法错误
8. ⚠️ 函数内的局部变量不能在函数外部直接使用

## 💻常用代码汇总

```
# 1. 固定次数循环
for _ in range(3):
    print("meow")

# 2. 输入校验：获取正整数
def get_positive():
    while True:
        n = int(input("请输入正整数："))
        if n > 0:
            return n

# 3. 列表遍历带序号（优化写法）
students = ["Harry", "Ron", "Hemin"]
for i, name in enumerate(students, 1):
    print(i, name)

# 4. 字典键值同时遍历（优化写法）
house = {"Harry": "Gran", "Ron": "Gran"}
for name, house_name in house.items():
    print(name, house_name)

# 5. 标准主函数结构
def main():
    n = get_positive()
    for _ in range(n):
        print("meow")

if __name__ == "__main__":
    main()
```

## 📎关联概念跳转

- [[CS50P｜Lecture 1 条件判断与布尔逻辑]]
- [[CS50P‑Lecture 3 函数进阶与异常处理]]
- [[Python 浮点数精度深度解析]]