# Cookie

## 一、什么是Cookie

1. Cookie翻译过来是饼干的意思，**由W3C组织提出**

2. 由于HTTP是一种无状态的协议，服务器想要知道客户端的身份，此时**Cookie应运而生**

3. **原理**：给客户端们颁发一个通行证每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了

4. 本质上：Cookie实际上是一小段的文本信息.

   > 客户端**请求**服务器，如果服务器需要**记录该用户状态**，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。

> **查看某个网站的Cookie**	**javascript:alert (document. cookie)**	

**Cookie的一些特性**

> **不可跨域名性**：根据Cookie规范，浏览器访问Google只会携带Google的Cookie，而不会携带Baidu的Cookie。Google也只能操作Google的Cookie，而不能操作Baidu的Cookie。
>
> 虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie

**Unicode编码：保存中文**

> Cookie中保存中文只能编码。一般使用UTF-8编码即可。不推荐使用GBK等中文编码，因为浏览器不一定支持，而且JavaScript也不支持GBK编码。

**设置Cookie的所有属性**

| 属 性 名       | 描  述                                                       |
| -------------- | ------------------------------------------------------------ |
| String name    | 该Cookie的名称。Cookie一旦创建，名称便不可更改               |
| Object value   | 该Cookie的值。如果值为Unicode字符，需要为字符编码。如果值为二进制数据，则需要使用BASE64编码 |
| **int maxAge** | **该Cookie失效的时间，单位秒。如果为正数，则该Cookie在maxAge秒之后失效。如果为负数，该Cookie为临时Cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该Cookie。如果为0，表示删除该Cookie。默认为–1** |
| boolean secure | 该Cookie是否仅被使用安全协议传输。安全协议。安全协议有HTTPS，SSL等，在网络上传输数据之前先将数据加密。默认为false |
| String path    | 该Cookie的使用路径。如果设置为“/sessionWeb/”，则只有contextPath为“/sessionWeb”的程序可以访问该Cookie。如果设置为“/”，则本域名下contextPath都可以访问该Cookie。注意最后一个字符必须为“/” |
| String domain  | 可以访问该Cookie的域名。如果设置为“.google.com”，则所有以“google.com”结尾的域名都可以访问该Cookie。注意第一个字符必须为“.” |
| String comment | 该Cookie的用处说明。浏览器显示Cookie信息的时候显示该说明     |
| int version    | 该Cookie使用的版本号。0表示遵循Netscape的Cookie规范，1表示遵循W3C的RFC 2109规范 |

**Cookie的有效期**

>  Cookie的**maxAge**决定着Cookie的有效期，单位为秒（Second）
>
> 
>
> | 区别：            |                                                              |
> | ----------------- | :----------------------------------------------------------: |
> | maxAge属性 为正数 | 表示该Cookie会在maxAge秒之后自动失效，会将maxAge为正数的Cookie持久化，即写到对应的Cookie文件中，只要还在maxAge秒之前，无论是关闭电脑还是浏览器，登录网站时该Cookie仍然有效 |
> | maxAge属性 为负数 | 表示该Cookie仅在本浏览器窗口以及本窗口打开的子窗口内有效，关闭窗口后该Cookie即失效，maxAge为负数的Cookie，为临时的Cookie不会被持久化，不会写到Cookie文件中 |
> | maxAge属性 为0    | 表示删除该Cookie。Cookie机制没有提供删除Cookie的方法，因此通过设置该Cookie即时失效实现删除Cookie的效果。失效的Cookie会被浏览器从Cookie文件或者内存中删除 |
>
>  **注：**response对象没有提供对Cookie操作的方法只有一个添加操作add(Cookie cookie),要想修改它，可以使		 用一个同名的Cookie来覆盖原来的Cookie，以此来达到删除原来Cookie的操作
> 	     在**删除**或**修改**的时候，新建的Cookie除**value、maxAge之外**的所有属性，都要与**原Cookie完全一样**。否则，浏览器将视为两个不同的Cookie不予覆盖，导致修改、删除失败。

**Cookie的域名**

​		Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。

　　正常情况下，同一个一级域名下的两个二级域名如www.helloweenvsfei.com和images.helloweenvsfei.com也不能交互使用Cookie，因为二者的域名并不严格相同。如果想所有helloweenvsfei.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数，

```java
Cookie cookie = new Cookie("time","20080808"); // 新建Cookie
cookie.setDomain(".helloweenvsfei.com");           // 设置域名
cookie.setPath("/");                              // 设置路径
cookie.setMaxAge(Integer.MAX_VALUE);               // 设置有效期
response.addCookie(cookie);   
```

> 可以修改本机C:\WINDOWS\system32\drivers\etc下的hosts文件来配置多个临时域名，然后使用setCookie.jsp程序来设置跨域名Cookie验证domain属性。
>
> 　　注意：domain参数必须以点(".")开始。另外，name相同但domain不同的两个Cookie是两个不同的Cookie。如果想要两个域名完全不同的网站共有Cookie，可以生成两个Cookie，domain属性分别为两个域名，输出到客户端。

**Cookie的路径**

> domain属性决定运行访问Cookie的域名，而path属性决定允许访问Cookie的路径（ContextPath）如果只允许/sessionWeb/下的程序使用Cookie可以这样写
>
> ```java
> Cookie cookie = new Cookie("time","20080808");     // 新建Cookie
> 
> cookie.setPath("/session/");                          // 设置路径
> 
> response.addCookie(cookie);                           // 输出到客户端
> ```
>
> ​		设置为“/”时允许所有路径使用Cookie。path属性需要使用符号“/”结尾。name相同但domain相同的两个Cookie也是两个不同的Cookie。
>
> 　　注意：页面只能获取它属于的Path的Cookie。例如/session/test/a.jsp不能获取到路径为/session/abc/的Cookie。使用时一定要注意。

**Cookie的安全属性**

> ​		HTTP协议不仅是无状态的，而且是不安全的。使用HTTP协议的数据不经过任何加密就直接在网络上传播，有被截获的可能。使用HTTP协议传输很机密的内容是一种隐患。如果不希望Cookie在HTTP等非安全协议中传输，可以设置Cookie的secure属性为true。浏览器只会在HTTPS和SSL等安全协议中传输此类Cookie。下面的代码设置secure属性为true：
>
> ```java 
> Cookie cookie = new Cookie("time", "20080808"); // 新建Cookie
> 
> cookie.setSecure(true);                           // 设置安全属性
> 
> response.addCookie(cookie);                        // 输出到客户端
> ```
>
> 提示：secure属性并不能对Cookie内容加密，因而不能保证绝对的安全性。如果需要高安全性，**需要在程序中对Cookie内容加密、解密，以防泄密。**

#### **下面的代码会输出本页面所有的Cookie。**

``` java 
<script>document.write(document.cookie);</script>
```

> 注：此代码读取不了出自己以外的Cookie