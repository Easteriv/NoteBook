<a name="vga7f"></a>
# 关于print与sys.stdout
在python中`print`语句实现打印，从技术角度来说，这是把一个或多个对象转换为其文本表达式形式，然后发送给标准输出流或者类似的文件流，更详细的说，打印与文件和流的概念紧密相连。

---

我们都知道在python中，向一个文件写东西是通过类似`file.write(str)`方法实现的，而你可能没想到print语句执行的操作其实也是一个写操作，不过他把我们从外设输入的数据写到了stdout流，并进行了一些特定的格式化。

:::warning
和文件方法不同，在执行打印操作是，不需要将对象转换为字符串（print已经帮我们做好了）
:::
```python
print(123)
```
等价于
```python
import sys

sys.stdout.write(str(123)+'\n')
```

这里的`sys.stdout`也就是我们python中标准输出流，这个标准输出流默认是映射到打开脚本的窗口的，所以，我们的print操作会把字符打印到屏幕上。既然sys.stdout默认是映射到打开脚本的窗口，那么这个映射关系是否可以修改呢？

答案是肯定的，这也是python中常用的一个小技巧，我们可以通过修改这种映射关系来把我们的打印操作重定向到其它地方，例如特定的文件。方法就是给sys.stdout赋值，修改它的指向。看下面的例子：

```python
import sys

sys.stdout = open('test.txt','w')
print 'Hello world'
```

可以看到，我们让sys.stdout指向了一个文件对象。然后，再执行打印操作，这时，hello world输出在了一个文件test.txt中：<br />但是，上面的代码有一个问题，我们把打印重定向到了一个文件中，那么在程序后面每一处调用print操作的输出都在这个文件中，那么我们后面想要打印字符到屏幕怎么办？

所以，这就需要我们先保存原始的sys.stdout，后面想要恢复的时候再赋值就行了，实现如下：
```python
import sys
temp = sys.stdout
sys.stdout = open('test.txt','w')
print 'hello world'
sys.stdout = temp #恢复默认映射关系
print 'nice'
```

`sys.stdout`除了可以映射到一个文件外，还有什么可以做的吗？当然有的，**你甚至可以将sys.stdout赋值为一个自定义的对象，前提是这个对象实现了write方法。**毕竟print调用的就是`sys.stdout.write()`方法。你可以自定义write方法，实现一些复杂的操作。

```python
class Test:
	def write(self,string):
		#do something you wanna do

test = Test()
temp = sys.stdout
sys.stdout = test
print 'hello world'
```
<a name="ljuAw"></a>
# 简单通过print实现日志打印收集
<a name="Q3PH8"></a>
## 自定义日志类
```python
class CustomLog(object):
    def __init__(self, fileN):
        self.terminal = sys.stdout
        self.log = open(fileN, 'w+', encoding='utf-8')


    def write(self, message):
        self.terminal.write(message)
        self.log.write(message)


    def flush(self):
        pass
```
<a name="hx5oe"></a>
## 代码调用
```python
sys.stdout = CustomLog(str(cwd) + r'/output.txt')
print("hello word")
```
