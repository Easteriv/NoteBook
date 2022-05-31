有时候去写爬虫,发现那个请求头是真的多,而且格式比较的复杂,在`python`中如果`header`比较多，那么在编写代码比较麻烦。但是我们可以利用`postman`来帮助我们简化这一过程，下文以百度为例：

1. 打开网页`https://www.baidu.com`
1. F12打开控制台，如下，点击你需要复制的url,右键选择`copy`-> `copy as curl(bash)`

![image.png](https://cdn.omsear.com/docsify/img/13.png)

3. 打开postman，顺序依次是`import`->`Raw text`->`continue`

![image.png](https://cdn.omsear.com/docsify/img/14.png)

4. 生成代码：选择代码块样式标签<>,然后选择生成的语言，本次以Python 示例

![image.png](https://cdn.omsear.com/docsify/img/15.png)
·
