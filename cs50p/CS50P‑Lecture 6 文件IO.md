

> 🔗 前置课程：[[CS50P-Lecture 5 单元测试，测试代码]]

## 一、基础文件操作

### 1. open 函数与文件模式

`open()` 函数返回一个**文件对象**，用于读写操作，需要用变量接收。 第二个参数指定操作模式：

|模式|全称|作用|文件不存在时|
|---|---|---|---|
|`"r"`|read|只读模式（默认）|抛出报错|
|`"w"`|write|覆盖写入|自动创建文件|
|`"a"`|append|追加写入（末尾添加）|自动创建文件|

> ⚠️ 注意：`"w"` 会直接清空覆盖原有全部内容，慎用。

### 2. 两种打开方式

#### 推荐：with 上下文管理器

自动关闭文件，即使代码中途报错也会正常关闭，避免资源泄漏，是CS50推荐写法。

```
names = input("who?")
with open("names.txt", "a") as file:
    file.write(f"{names}\n")
```

#### 手动 close（不推荐）

需要手动调用关闭，忘记关闭会造成文件资源占用。

```
names = input("who?")
file = open("names.txt", "a")
file.write(f"{names}\n")
file.close()
```

### 3. 读取文件的几种方式

#### 方式1：readlines() 读取所有行

返回一个列表，每个元素对应文件中的一行。

```
with open("names.txt", "r") as file:
    lines = file.readlines()

for line in lines:
    print("hello", line.rstrip())
```

- `line.rstrip()`：去除行尾的换行符 `\n`；也可以用 `print("hello", line, end="")` 实现同样效果。

#### 方式2：直接迭代文件对象

文件对象本身是可迭代的，逐行遍历，大文件更省内存。

```
with open("names.txt", "r") as file:
    for line in file:
        print("hello", line.rstrip())
```

#### 易混区分：read / readline / readlines

|方法|返回值|作用|
|---|---|---|
|`file.read()`|单个字符串|一次性读取文件全部内容|
|`file.readline()`|单个字符串|只读取一行内容|
|`file.readlines()`|列表|读取所有行，每行是列表一个元素|

### 4. 写入文件

使用 `文件对象.write(内容)` 写入，内容必须是字符串类型，换行需要手动加 `\n`。

---

## 二、文件内容排序

`sorted()` 可以对任何可迭代对象排序，不会修改原文件，只返回排序后的结果。

### 1. 读取到列表再排序（推荐，灵活）

先把内容存入列表，对列表排序后输出。

```
names = []
with open("names.txt", "r") as file:
    for name in file:
        names.append(name.rstrip())

for line in sorted(names):
    print("hello,", line)
```

### 2. 直接对文件对象排序

文件对象可直接迭代排序，写法更简洁。

```
with open("names.txt", "r") as file:
    for line in sorted(file):
        print("hello,", line.rstrip())
```

### 3. sorted 函数补充

- 第一个参数：可迭代对象（列表、文件对象等）
- `reverse=False`：默认升序（a~z）；设为 `True` 则降序（z~a）
    
    ```
    sorted(file, reverse=True)
    ```
    
- `key=函数`：指定排序依据，字典排序会用到。

---

## 三、CSV 文件处理

CSV 即逗号分隔值（Comma-Separated Values），是表格数据常用存储格式。

### 1. 手动 split 拆分（仅简单场景）

每行没有内嵌逗号时，可以用 `split(",")` 拆分。

```
with open("students.csv") as file:
    for line in sorted(file):
        name, house = line.rstrip().split(",")
        print(f"{name} is in {house}")
```

> 局限：如果字段内容里本身包含逗号（用引号包裹），split 会拆分错误。

### 2. csv 标准库（推荐）

Python 内置 `csv` 库，专门处理CSV格式，自动识别引号包裹的逗号。

#### csv.reader：按列表读取

每一行返回一个列表，按索引取值。

```
import csv

students = []
with open("students.csv") as file:
    reader = csv.reader(file)
    for name, house in reader:
        students.append({"name": name, "house": house})

for stu in sorted(students, key=lambda a: a["name"]):
    print(f"{stu['name']} is in {stu['house']}")
```

#### csv.DictReader：按字典读取（带表头）

CSV第一行作为表头，自动成为字典的键，按列名取值，可读性更强。

```
import csv

students = []
with open("students.csv") as file:
    reader = csv.DictReader(file)
    for row in reader:
        students.append({
            "name": row["name"],
            "house": row["house"],
            "title": row["title"]
        })

for stu in sorted(students, key=lambda a: a["name"]):
    print(f"{stu['name']} is in {stu['house']} and title is {stu['title']}")
```

对应CSV文件示例：

```
name,house,title
Ron,"Gran,111",b
Harry,Gran,c
Hemin,Gran,d
Draco,Slai,e
```

### 3. 写入 CSV 文件

#### csv.writer：按列表写入

```
import csv

name = input("what's your name?")
home = input("where are you from")

with open("write1.csv", "a", newline="") as file:
    writer = csv.writer(file)
    writer.writerow([name, home])
```

> 小技巧：加 `newline=""` 可以避免Windows下写入出现多余空行。

#### csv.DictWriter：按字典写入

指定列顺序，按字典写入，适合多列场景。

```
import csv

name = input("what's your name?")
home = input("where are you from")

with open("write1.csv", "a", newline="") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "home"])
    # 首次写入需要加这行写入表头
    # writer.writeheader()
    writer.writerow({"name": name, "home": home})
```

---

## 四、排序进阶：字典排序与 key 参数

当列表元素是字典时，需要通过 `key` 参数指定排序依据的字段。

### 1. 自定义函数作为 key

```
students = []
with open("students.csv") as file:
    for line in file:
        name, house = line.rstrip().split(",")
        student = {"name": name, "house": house}
        students.append(student)

def get_name(a):
    return a["name"]

for stu in sorted(students, key=get_name):
    print(f"{stu['name']} is in {stu['house']}")
```

- 原理：`sorted` 遍历列表时，会把每个元素传给 `get_name` 函数，用返回值作为排序依据。
- 注意：`key=` 后面只写函数名，**不要加括号**，加括号会直接执行函数导致报错。

### 2. lambda 匿名函数（更简洁）

单行函数可以用 `lambda` 简写，效果完全一致。

```
for stu in sorted(students, key=lambda a: a["name"]):
    print(f"{stu['name']} is in {stu['house']}")
```

语法：`lambda 参数: 返回值`

---

## 五、PIL 图像处理（生成GIF）

PIL 是Python第三方图像处理库，安装包名为 `pillow`，代码导入名为 `PIL`。

### 1. 安装与导入

```
pip install pillow
```

```
import sys
from PIL import Image
```

> ⚠️ 经典坑：安装包名 `pillow` ≠ 导入名 `PIL`；`Image` 类首字母必须大写。

### 2. 生成GIF示例

通过命令行传入多张图片，合成动图。

```
import sys
from PIL import Image

images = []
# 遍历命令行参数（跳过脚本文件名）
for arg in sys.argv[1:]:
    img = Image.open(arg)
    images.append(img)

# 用第一张图作为基底，追加后续图片
images[0].save(
    "cat.gif",
    save_all=True,
    append_images=[images[1]],
    duration=200,
    loop=0
)
```

### 3. 参数说明

- `save_all=True`：保存所有帧，不是只保存第一帧
- `append_images=[]`：要追加的图片列表
- `duration=200`：每帧停留时间，单位毫秒
- `loop=0`：循环次数，0表示无限循环

---

## 六、常见易错点汇总

1. **变量名覆盖内置关键字**：不要用 `list`、`file`、`str` 等Python内置名称做变量名，建议用 `students`、`names_list` 替代。
2. **文件模式记错**：`"r"` 模式文件不存在会报错，只有 `"w"`/`"a"` 会自动创建。
3. **写入忘记加换行**：`write()` 不会自动加 `\n`，需要手动补充。
4. **sorted 的 key 加括号**：`key=函数名` 即可，加括号会立即执行，导致报错。
5. **PIL 大小写错误**：`from PIL import Image`，PIL和Image都要注意大小写。
6. **CSV写入空行**：Windows下打开文件加 `newline=""` 可避免。
7. **直接修改原文件**：`sorted()` 只返回排序后的结果，不会修改源文件。