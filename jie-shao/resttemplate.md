# RestTemplate

官方教程： [https://spring.io/guides/gs/consuming-rest/](https://spring.io/guides/gs/consuming-rest/)

[http://www.baeldung.com/rest-template](http://www.baeldung.com/rest-template)

[http://rensanning.iteye.com/blog/2362105](http://rensanning.iteye.com/blog/2362105)



经常需要发送一个GET/POST请求到其他系统\(REST API\)，通过JDK自带的HttpURLConnection、Apache HttpClient、Netty 4、OkHTTP 2/3都可以实现。   
  
Spring的RestTemplate封装了这些库的实现，使用起来更简洁。   
  
RestTemplate包含以下几个部分：   


* HttpMessageConverter 对象转换器
* ClientHttpRequestFactory 默认是JDK的HttpURLConnection
* ResponseErrorHandler 异常处理
* ClientHttpRequestInterceptor 请求拦截器

