有时候去写爬虫,发现那个请求头是真的多,而且格式比较的复杂,在`python`中如果`header`比较多，那么在编写代码比较麻烦。但是我们可以利用`postman`来帮助我们简化这一过程，下文以百度为例：

1. 打开网页`https://www.baidu.com`
1. F12打开控制台，如下，点击你需要复制的url,右键选择`copy`-> `copy as curl(bash)`

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1385618/1653978528822-ade2b29d-cc6b-48c2-9278-ca0cf09ae185.png#clientId=uc2df7650-f622-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=853&id=u5753d50f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=853&originWidth=1697&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101022&status=done&style=none&taskId=u1f885c7e-bafc-40f1-9314-c492295b567&title=&width=1697)

3. 打开postman，顺序依次是`import`->`Raw text`->`continue`

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1385618/1653978377653-1ff3520d-c734-4103-aef9-2ebb2bc2c5fa.png#clientId=uc2df7650-f622-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=804&id=uae75ac7f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=804&originWidth=1634&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91254&status=done&style=none&taskId=u8431e891-789e-46cd-b8e5-bcd422b4520&title=&width=1634)

4. 生成代码：选择代码块样式标签<>,然后选择生成的语言，本次以Python 示例

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1385618/1653978735561-c2c45c54-efa0-4a57-a88a-0d2e2df5445e.png#clientId=uc2df7650-f622-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=803&id=u108ee65b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=803&originWidth=1539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=110868&status=done&style=none&taskId=ud356ddfa-5174-4f14-ab8f-9beb4183d74&title=&width=1539)
·
