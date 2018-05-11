# https

参考：

[https://letsencrypt.org/getting-started/](https://letsencrypt.org/getting-started/)

{% embed data="{\"url\":\"http://www.ruanyifeng.com/blog/2014/02/ssl\_tls.html\",\"type\":\"link\",\"title\":\"SSL/TLS协议运行机制的概述 - 阮一峰的网络日志\",\"icon\":{\"type\":\"icon\",\"url\":\"http://www.ruanyifeng.com/favicon.ico\",\"aspectRatio\":0}}" %}

[http://www.ruanyifeng.com/blog/2016/08/migrate-from-http-to-https.html](http://www.ruanyifeng.com/blog/2016/08/migrate-from-http-to-https.html)

SSL/TLS协议的基本思路是采用[公钥加密法](http://en.wikipedia.org/wiki/Public-key_cryptography)，也就是说，客户端先向服务器端索要公钥，然后用公钥加密信息，服务器收到密文后，用自己的私钥解密。



5.1 HTTP Strict Transport Security \(HSTS\)

访问网站时，用户很少直接在地址栏输入`https://`，总是通过点击链接，或者3xx重定向，从`HTTP`页面进入`HTTPS`页面。攻击者完全可以在用户发出`HTTP`请求时，劫持并篡改该请求。

另一种情况是恶意网站使用自签名证书，冒充另一个网站，这时浏览器会给出警告，但是许多用户会忽略警告继续访问。

"HTTP严格传输安全"（简称 HSTS）的作用，就是强制浏览器只能发出`HTTPS`请求，并阻止用户接受不安全的证书。

它在网站的响应头里面，加入一个强制性声明。以下例子摘自[维基百科](https://zh.wikipedia.org/wiki/HTTP%E4%B8%A5%E6%A0%BC%E4%BC%A0%E8%BE%93%E5%AE%89%E5%85%A8)。

> ```text
>
> Strict-Transport-Security: max-age=31536000; includeSubDomains
> ```

上面这段头信息有两个作用。

> （1）在接下来的一年（即31536000秒）中，浏览器只要向`example.com`或其子域名发送HTTP请求时，必须采用HTTPS来发起连接。用户点击超链接或在地址栏输入`http://www.example.com/`，浏览器应当自动将`http`转写成`https`，然后直接向`https://www.example.com/`发送请求。
>
> （2）在接下来的一年中，如果`example.com`服务器发送的证书无效，用户不能忽略浏览器警告，将无法继续访问该网站。

HSTS 很大程度上解决了 SSL 剥离攻击。只要浏览器曾经与服务器建立过一次安全连接，之后浏览器会强制使用`HTTPS`，即使链接被换成了`HTTP`。

该方法的主要不足是，用户首次访问网站发出HTTP请求时，是不受HSTS保护的。

如果想要全面分析网站的安全程度，可以使用 Mozilla 的 [Observatory](https://observatory.mozilla.org/)。  
  




