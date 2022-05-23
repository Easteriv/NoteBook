登录方式主要有两种：

1. 直接携带cookies
1. 找url地址，发送post请求存储cookie

<a name="pKBaJ"></a>
# 直接携带cookies
cookie只能传递给cookies参数接收,示例：
```python
   yield scrapy.Request(
            self.start_urls[0],
            callback=self.parse,
            cookies=cookies_dict
        )
```
<a name="ze2Gw"></a>
# post请求传递cookies
本次示例爬取`http://001.0f1.cn/`

1. 分析登录请求

![image.png](https://cdn.omsear.com/docsify/img/09.png)

2. 抓取登录请求url
```python
 login_url = response.xpath("//form[@name='login']/@action").extract_first()
```

3. post模拟登录
```python
    def parse_login_url(self, response):
        login_url = response.xpath("//form[@name='login']/@action").extract_first()
        param = {
            "referer": "http://001.0f1.cn/",
            "name": "hello123",
            "password": "123456"
        }
        yield scrapy.FormRequest(url=login_url,
                                 callback=self.parse_login, formdata=param)
```

4. 验证cookies是否生效
- 可以在`setting.py`中设置`COOKIES_DEBUG = True`查看cookies传递情况

![08.png](https://cdn.omsear.com/docsify/img/08.png)

- 抓取页面查看个人介绍，如果存在，表明已经登录
```python
  def parse(self, response, **kwargs):
        item = SpimesspiderItem()
        introduction_data = response.xpath("//div[@class='header-box']//p/text()").extract()
        if len(introduction_data) != 0:
            item['introduction'] = introduction_data[0]
        yield item
```
运行结果，可以看到cookies传递成功
```python
{'introduction': '作者有点忙，还没写简介 '}
```
