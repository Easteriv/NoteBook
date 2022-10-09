# 一.简洁之美
通过⼀⾏代码，体会Python语⾔简洁之美

1.  ⼀⾏代码交换 a , b 
```python
a, b = b, a
```

2. ⼀⾏代码反转列表
```python
"""
使用模式: [start:end:step]
    其中start表示切片开始的位置,默认是0
    end表示切片截止的位置(不包含),默认是列表长度
    step表示切片的步长,默认是1
    当start是0时,可以省略;当end是列表的长度时,可以省略.
    当step是1时,也可以省略,并且省略步长时可以同时省略最后一个冒号.
    此外,当step为负数时,表示反向切片,这时start值应该比end值大.
    注意:切片操作创建了一个新的列表.
"""
[1,2,3][::-1] # [3,2,1]
```

3. ⼀⾏代码合并两个字典
```python
"""
字典拆包
"""
{**{'a':1,'b':2}, **{'c':3}} # {'a': 1, 'b': 2, 'c': 3}
```

4. ⼀⾏代码列表去重
```python
"""
set中可以传参，必须是可迭代对象，或者为空，那么默认就是一个空集合对象
"""
set([1,2,2,3,3,3]) # {1, 2, 3}
```

5. ⼀⾏代码求多个列表中的最⼤值
```python
"""
max() 函数返回有最大值的项目，或者 iterable 中有最大值的项目。
如果值是字符串，则按字母顺序进行比较。
"""
max(max([ [1,2,3], [5,1], [4] ], key=lambda v: max(v))) # 5
```

6. ⼀⾏代码⽣成逆序序列
```python
list(range(10,-1,-1)) # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
# 二.基础知识

1. 求绝对值
```python
abs(-6) #6
```

2. 元素都为真
```python
"""
接受⼀个迭代器，如果迭代器的 所有元素 都为真，那么返回 True，否则返回 False
参数：元组或列表
注意：空元组，空列表返回True
"""
all([1,0,3,6]) #False
all([1,3,6]) #True
```

3. 元素⾄少⼀个为真
```python
""""
接受⼀个迭代器，如果迭代器⾥ ⾄少有⼀个 元素为真，那么返回 True，否则返回 False
"""
any([0,0,0,[]]) # False
any([0,0,1]) # True
```

4. __repr__方法，它是一个 ”自我描述“ 的方法，此方法通常实现这样的功能： 当直接打印类的实例化对象时，系统将会输出对象的自我描述信息，用来告诉外界对象具有的状态信息。
```python
class student(object):
    
    def __init__(self,name,age):
        self.name = name
        self.age = age

    def __repr__(self):
        return f'Student name is {self.name},age is {self.age}'
if __name__ == '__main__':
    a = student('张三',14)
    print(a) # Student name is 张三,age is 14
```

5. 判断真假
```python
"""
bool() 函数用于将给定参数转换为布尔类型，如果没有参数，返回 False。
bool 是 int 的子类
"""
bool(0) # False
bool(1) # True
```

6. 字符串与字节转换
```python
test_str = "hello"
b = bytes(test_str,encoding='utf-8') # b'hello'
s = str(b,'utf-8') # 'hello'
```

7. 是否可调⽤
```python
"""
callable() 函数用于检查一个对象是否是可调用的。如果返回 True，object 仍然可能调用失败；
但如果返回 False，调用对象 object 绝对不会成功。
对于函数、方法、lambda 函式、 类以及实现了 __call__ 方法的类实例, 它都返回 True
"""
callable(str) #True
```

8. 类方法
```python
"""
classmethod 装饰器对应的函数不需要实例化，不需要 self参数，但第⼀个参数需要是表⽰⾃⾝类
的 cls 参数，可以来调⽤类的属性，类的⽅法，实例化对象等
"""
class Student(object):
    @classmethod
    def getClass(cls):
        print(cls)
if __name__ == '__main__':
    Student.getClass() # <class '__main__.Student'>
```

9. 动态删除属性
```python
class Student(object):
    def __init__(self):
        self.age = 13
if __name__ == '__main__':
    s = Student()
    delattr(s,'age')
    has_ = hasattr(s,'age')
    print(has_) # False
```

10. 转为字典
```python
a = dict()
print(a) #{}
b = dict(x='1',y='2')
print(b) #{'x': '1', 'y': '2'}
c = dict(zip(['a','b'],[3,4]))
print(c) #{'a': 3, 'b': 4}
d = dict([('a',1),('b',2)])
print(d) # {'a': 1, 'b': 2}
```

11. ⼀键查看对象所有⽅法
```python
"""
dir() 函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；
带参数时，返回参数的属性、方法列表
"""
class Student(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def say(self):
        return f'My name is :{self.name}'
if __name__ == '__main__':
    print(dir(Student))
```

12. 取商和余数
```python
a,b= divmod(10,3)
print(a,b) # 3 1
```

13. enumerate函数
```python
"""
这个函数的基本应用就是用来遍历一个集合对象，它在遍历的同时还可以得到当前元素的索引位置
索引值默认从0开始，但也可以将其设置为任何整数
"""
l = [1,2,3,4]
for index,value in enumerate(l):
     print(f'index:{index},value is :{value}') 
        # index:0,value is :1
        # index:1,value is :2
        # index:2,value is :3
        # index:3,value is :4
```

14. 计算表达式
```python
s = '1+2+3'
print(s) # 1+2+3
print(eval(s)) # 6
```

15. `fliter`过滤器
```python
l = [1, 2, 3, 4]
res = filter(lambda x: x > 3, l)
print(list(res)) # [4]
```

16. 冻结集合
```python
frozenset(['b', 'r', 'u', 'o', 'n'])   # 创建不可变集合
```

17. 动态获取对象属性
```python
class Student(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
if __name__ == '__main__':
    s = Student(name='hangman', age=12)
    print(getattr(s, 'age')) # 12
```
