<a name="KWhbE"></a>
# 一、简易爬虫的编写
<a name="MzSwS"></a>
## 安装scrapy
```bash
pip/pip3 install scrapy
```
<a name="yuQmX"></a>
## 创建项目

- 本次以`yuleSpider`为例（项目名字可以自定义）
```bash
scrapy startproject yuleSpider
```
创建成功截图<br />![image.png](https://zhaojiejun.oss-cn-hangzhou.aliyuncs.com/docsify/img/image.png)
<a name="fkVWM"></a>
## 创建爬虫

- scrapy genspider[爬虫名字][允许爬取的域名]
```python
scrapy genspider yule news.yule.com.cn
```
执行成功<br />![image.png](https://cdn.omsear.com/docsify/img/01.png)
<a name="R3UVd"></a>
## 解析页面

- 在`yule.py`编写页面解析逻辑，本文以[http://news.yule.com.cn/yingshi/](http://news.yule.com.cn/yingshi/)为例

![解析逻辑](https://cdn.omsear.com/docsify/img/06.png)
- scrapy.Spider爬虫类中必须有名为parse的解析
- 如果网站结构层次比较复杂，也可以自定义其他解析函数
- 在解析函数中提取的url地址如果要发送请求，则必须属于allowed_domains范围内，但是start_urls中的url地址不受这个限制
- 启动爬虫的时候注意启动的位置，是在项目路径下启动
- parse()函数中使用yield返回数据，**注意：解析函数中的yield能够传递的对象只能是：BaseItem, Request, dict, None**

<a name="yVFC9"></a>
## 数据清洗

- 数据管道`pipelines.py`中编写数据清洗逻辑  本文暂时保存在文本文件中，后续可替换至数据库等存储中间件

![数据清洗](https://cdn.omsear.com/docsify/img/03.png)

## 启用管道

- 在`settings.py`中启用管道配置
<a name="BV6EJ"></a>
## 运行爬虫
```bash
scrapy crawl yule --nolog
```




- 配置项中键为使用的管道类，管道类使用.进行分割，第一个为项目目录，第二个为文件，第三个为定义的管道类
- 配置项中值为管道的使用顺序，设置的数值约小越优先执行，该值一般设置为1000以内

<a name="dxXL8"></a>
# 二、翻页请求
<a name="RAU4C"></a>
## 数据建模


1. 定义item即提前规划好哪些字段需要抓，防止手误，因为定义好之后，在运行过程中，系统会自动检查
1. 配合注释一起可以清晰的知道要抓取哪些字段，没有定义的字段不能抓取，在目标字段少的时候可以使用字典代替
1. 使用scrapy的一些特定组件需要Item做支持，如scrapy的ImagesPipeline管道类

在`items.py`文件中定义要提取的字段，示例：

![数据建模](https://cdn.omsear.com/docsify/img/04.png)

## 翻页请求
在`yule.py`文件编写翻页代码

![完善翻页](https://cdn.omsear.com/docsify/img/05.png)

## 关于scrapy.Request

```python
scrapy.Request(url[,callback,method="GET",headers,body,cookies,meta,dont_filter=False])
```
<a name="AP2fK"></a>
### 参数解释

- 中括号里的参数为可选参数
- callback：表示当前的url的响应交给哪个函数去处理
- meta：实现数据在不同的解析函数中传递，meta默认带有部分数据，比如下载延迟，请求深度等
- dont_filter:默认为False，会过滤请求的url地址，即请求过的url地址不会继续被请求，对需要重复请求的url地址可以把它设置为Ture，比如贴吧的翻页请求，页面的数据总是在变化;start_urls中的地址会被反复请求，否则程序不会启动
- method：指定POST或GET请求
- headers：接收一个字典，其中不包括cookies
- cookies：接收一个字典，专门放置cookies
- body：接收json字符串，为POST的数据，发送payload_post请求时使用）

<a name="l9sZc"></a>
### meta参数
> meta的作用：meta可以实现数据在不同的解析函数中的传递

在爬虫文件的parse方法中，提取详情页增加之前callback指定的parse_detail函数：
<a name="9bb73523"></a>
##### 特别注意

- meta参数是一个字典
- meta字典中有一个固定的键proxy，表示代理ip

## Meta传递参数
- 数据建模添加字段
- 构建详情页GET请求

![最终代码](https://cdn.omsear.com/docsify/img/07.png)

- yield scrapy.Request(url, callback=self.parse_detail, meta={})
- 利用meta参数在不同的解析函数中传递数据:
- 通过前一个解析函数 yield scrapy.Request(url, callback=self.xxx, meta={}) 来传递meta
- 在self.xxx函数中 response.meta.get('key', '') 或 response.meta['key'] 的方式取出传递的数据
