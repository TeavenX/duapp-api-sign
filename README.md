# duapp-api-sign 毒APP newSign算法分析
毒APP主要使用`sign`和`newSign`两个参数来对url进行验签，下面是首页推荐接口的请求：
```
GET https://app.poizon.com/api/v1/app/index/ice/shopping?lastId=&limit=20&newSign=3b799d62162b6ce9101f2b4eecee2e46](https://app.poizon.com/api/v1/app/index/ice/shopping?lastId=&limit=20&newSign=3b799d62162b6ce9101f2b4eecee2e46 HTTP/1.1
duuuid: 124ed23e39b48f13
duimei: 869437022872919
duplatform: android
appId: duapp
duchannel: du
duv: 4.16.1
duloginToken: c651c053|71979198|c1d97f25f2e7b844
dudeviceTrait: 2014813
timestamp: 1573811922502
shumeiid: 201911060919363513942e835afe4a655e595011a6c1f2018cae614d9493b0
User-Agent: duapp/4.16.1(android;5.1.1)
Host: app.poizon.com
Connection: Keep-Alive
Accept-Encoding: gzip
Cookie: duToken=d41d8cd9%7C71979198%7C1573801295%7C82764f7dbf13b384
```
`sign`的计算方法如下：  
1. 把 url中的参数放入map中
2. 把`uuid、platform、v、loginToken `放入map中
3. 对map中的元素按key进行排序
4. 把map中的元素按keyvalue形式拼接成字符串
5. 在字符串末尾拼接一个常量字符串
6. 计算字符串的md5
![屏幕快照 2019-11-15 下午6.03.25.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDc3NDk5Mi1kODg4ZDJjZGU5MjkyOTJiLnBuZw?x-oss-process=image/format,png)  
`newSign`的计算方法跟`sign`比较相似，只是最后是先对构建的字符串AES加密，然后再计算md5，加密部分是在`libJNIEncrypt.so`中实现。
![屏幕快照 2019-11-25 上午10.32.58.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDc3NDk5Mi1iNTliMzUzMzAwMTM1Njg3LnBuZw?x-oss-process=image/format,png)     
毒APP API的签名参数计算流程就这样，对细节感兴趣的朋友可以[联系](http://47.105.95.219)。
