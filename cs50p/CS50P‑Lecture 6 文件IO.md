open函数会返还一个特殊的值，用这个值去读取文件，所以需要变量去接收,open函数中如果文件不存在还会自动创建这个文件，括号内第一个参数为文件名，第二个参数为对文件进行的操作eg: "r"read读    "a"append添加
"w"write写与添加不同，写回直接覆盖之前的内容

with指定在某个上下文环境中，打开文件并自动关闭他

```python
names = input("who?")

with open("names.txt","a") as file:

    file.write(f"{names}\n")
```

另一种关闭方式用file.close()
```python
names = input("who?")

file = open("names.txt","a")

file.write(f"{names}\n")

file.close()
```

读取文件
```python
with open("names.txt","r") as file:

    lines = file.readlines()

for line in lines:

    print("hello",line.rstrip())
```
line.rstrip()去除末尾多余的换行也可以用end=""
注意：readline和readlines并不一样
有s的是读取每一行，而没有s的是只读取第一行并且一个字母一个字母的读取

另一种读取文件的方法
```python
with open("names.txt","r") as file:

    for line in file:

        print("hello",line.rstrip())
```
直接使用一个for循环去读取文件中的每一行

如果想要对文档进行排序怎么办，就需要先建一个列表，将文档中的内容拷贝进去一份，并对列表进行排序，最后再打印列表即可，就像是c中引入了一个临时变量
```python
names = []

with open("names.txt","r") as file:

    for name in file:

        names.append(name.rstrip())

for line in sorted(names):

    print("hello,",line)
```
sorted()可以按照字母顺序，整理列表

还有更简单的排序方法
```python
with open("names.txt","r") as file:

    for line in sorted(file):

        print("hello,",line.rstrip())
```
直接对文件进行排序，当然这不会改变源文件
```python
sorted(file,reverse=True)
```
sorted函数中第一个参数是可迭代对象即可以被循环遍历的东西，reverse默认=False，如果改成True就会从z~a反向排序

程序员用CSV格式“逗号分隔值”Comma-Separated Values
```python
with open ("students.csv") as file:

    for line in sorted(file):

        row = line.rstrip().split(",")    #split分开后的值储存在一个列表    

        print(f"{row[0]} is in {row[1]}")
```
csv文件后面不需要加上以什么方式打开
split分开后的值储存在一个列表    

```python
name,house = line.rstrip().split(",")
```
如果明知道split分开后会有两个值，就可以直接赋个两个变量


之前是以每一行的第一个字母来排序的，现在想以名字或学院来排序
```python
list = []

with open ("students.csv") as file:

    for line in file:

        name,house = line.rstrip().split(",")

        student = {"name" : name, "house":house}
        #创建字典，并把两个变量作为值

        list.append(student)

  

def  get_name(a):

    return a["name"]

  

for stu in sorted(list,key=get_name):

    print(f"{stu['name']} is in {stu['house']}")
```
sorted函数中的key=.... 其实质是sorted函数在遍历list时，遇到第一个元素即字典，会把这个字典赋给get_name这个函数中的参数
get_name后面不能加括号，这样才能调用函数

还可以更简便，不去新写函数  、匿名函数
```python
list = []

with open ("students.csv") as file:

    for line in file:

        name,house = line.rstrip().split(",")

        student = {"name" : name, "house":house}

        list.append(student)

  
  

for stu in sorted(list,key=lambda a:a["name"]):

    print(f"{stu['name']} is in {stu['house']}")
```
key=lambda a : a["name"]
lambda是一个关键词 a是参数，冒号后面是要返回的值