# 网络基础知识

### http和https请求的区别

（1）https是http的安全版，http是由ssl+http协议构建的可进行加密传输、身份认证的网络协议。【安全版】

（2）默认端口不同，http默认端口是80，https默认端口是443【端口】

（3）http是明文传输，https是附带有安全性的ssl加密传输协议【协议】

（4）https需要到ca申请证书，需要一定费用。而且【成本】

（5）https握手阶段比较费时，对网站响应速度有影响【响应耗时】

### 浏览器对https证书进行认证的目的

- **校验双方身份的真实性**
- **数据的保密性**
- **数据的完整性**

### https通信的大致过程

> 原文链接：https://blog.csdn.net/zwjemperor/article/details/80719427

1. 客户端将自己支持的加密算法发送给服务器，**请求服务器证书**；

2. 服务器选取一组加密算法，并**将证书和公钥返回给客户端**；

3. **客户端校验证书合法性，生成随机对称密钥，用公钥加密后发送给服务器**；
4. **服务器用私钥解密出对称密钥，返回一个响应，HTTPS连接建立完成**；
5. **随后双方通过这个对称密钥进行安全的数据通信**。

### 三次握手的目的是什么

为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。节省服务端资源。

### 四次挥手的目的是什么

TCP是全双工模式，必须客户端和服务端都要收到对方发来的FIN，才会结束TCP连接

### http1.0、http1.1、http2.0的最主要区别是什么

> 原文链接：https://juejin.im/entry/5981c5df518825359a2b9476

最主要的区别是：性能提升

http1.0：每一次请求-响应，都需要建立一个TCP连接

http1.1：建立一次连接，可以串行化请求-响应，并且支持websocket

http2.0：建立一次连接，多个请求可以并行执行

### Charles抓取https流程

> 原文链接：https://blog.csdn.net/zwjemperor/article/details/80719427

1. 客户端向服务器发起HTTPS请求

2. Charles拦截客户端的请求，伪装成客户端向服务器进行请求
3. 服务器向“客户端”（实际上是Charles）返回服务器的CA证书

4. Charles拦截服务器的响应，获取服务器证书公钥，然后自己制作一张证书，将服务器证书替换后发送给客户端。（这一步，Charles拿到了服务器证书的公钥）

5. 客户端接收到“服务器”（实际上是Charles）的证书后，生成一个对称密钥，用Charles的公钥加密，发送给“服务器”（Charles）

6. Charles拦截客户端的响应，用自己的私钥解密对称密钥，然后用服务器证书公钥加密，发送给服务器。（这一步，Charles拿到了对称密钥）

7. 服务器用自己的私钥解密对称密钥，向“客户端”（Charles）发送响应

8. Charles拦截服务器的响应，替换成自己的证书后发送给客户端

9. 至此，连接建立，Charles拿到了 服务器证书的公钥 和 客户端与服务器协商的对称密钥，之后就可以解密或者修改加密的报文了。

简单来说，就是Charles作为**“中间人代理”**，拿到了 **服务器证书公钥** 和 **HTTPS连接的对称密钥**，前提是客户端选择信任并安装Charles的CA证书，否则客户端就会“报警”并中止连接。这样看来，HTTPS还是很安全的。

### GET请求和POST请求的区别？

GET请求：

（1）场景：常用于获取资源的请求

（2）请求字符串是拼接在url后面

（3）请求url长度有限制（2048个字符），所以请求字符串也是有长度限制的

（4）安全性相对较差（因为能在url后边看到请求参数）

（5）只能进行url编码

（6）参数的数据类型，GET只支持ASCII字符

POST请求：

（1）场景：常用语表单提交、创建数据、上传数据等

（2）请求参数放在请求体里

（3）请求参数没有长度限制

（4）安全性相对较好

（5）可以支持多种编码方式

（6）参数的数据类型，没有限制

### OSI七层模型

> 原文链接：https://blog.csdn.net/yaopeng_2005/article/details/7064869

**应用层、表示层、会话层、传输层、网络层、数据链路层、物理层**

![七层.jpg](http://ww1.sinaimg.cn/large/00788Gqbgy1g6vygtnqgfg30v41830yk.jpg)

### Cookie和Session的区别

Cookie和Session一般是搭配起来使用的。

**Cookie 是浏览器保存用户信息的文件**，存储的字符数有限制：**4096个字符**，Cookie的安全性不高，任何人都可以直接查看。

**Session 是指我们访问网站的一个会话周期**，Session的存储没有限制，一般会在Set-Cookie阶段，把SessionID放入到Cookie中，当浏览器下次访问服务器时，Cookie中会附带SessionID，服务器通过SessionID去Session表查看用户访问信息。

Cookie和Session的交互流程：

![cookie和session.jpg](http://ww1.sinaimg.cn/large/00788Gqbgy1g7997pwexqj30vp0hlgmi.jpg)

