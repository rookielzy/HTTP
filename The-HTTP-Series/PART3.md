# THE HTTP SERIES(PART 3): CLIENT IDENTIFICATION

从前两章里，我们学习了HTTP的基本概念和结构，我们将继续探讨下一个重要的知识点：用户识别。

在这章里，我们将学习为什么客户认证如此重要，同时Web服务器如何识别我们（我们的Web客户端），还会学习到信息是如何被使用的和存储的。

>* Client identification and why it’s extremely important
>* Different ways to identify the client
>* HTTP request headers used for identification
>* IP address
>* Long (fat) URLs
>* Cookies

首先，让我们来看看为什么网站需要识别我们。

## Client identification and why it’s extremely important 用户识别及其重要性

正如你所知，每个网站或多或少会关心你和你的行为，比如某些形式的个性化内容。

这是什么意思呢？

这些内容包括了当你访问了电商网站，它会推荐一些商品给你，或者在一些社交网络上推荐“你可能认识或想认识的人”，或者推荐一些视频，广告刚好就符合你的需要，或者一些新闻文章刚好与你有关等等。

这感觉像是双刃剑。一方面，将一些个性化的自定义内容推荐给你确实很不错；但是另一方面，它可能会导致各种先入为主的观念和偏见。

然而，更懂我们自己的还是自己。

无论哪种方式，内容个性化已经成为我们日常生活中的一部分，我们不可能简简单单的无视它。

接下来，然我们来看看Web服务器如何识别你从而达到上述的效果。

### Different ways to identify the client 几种不同的识别方式

<div align="center">
    <img src="img/multipass-768x320.jpg">
</div>

>* <b>HTTP request headers HTTP请求头</b>
>* <b>IP address IP地址</b>
>* <b>Long URLs</b>
>* <b>Cookies</b>
>* <b>Login information (authentication) 登录信息，验证信息</b>

让我们一个一个来，其中HTTP验证将会在下一章里进行详细描述。

## HTTP request headers used for identification 用HTTP请求头来识别

Web服务器有几种方法来从HTTP请求头获取关于你的信息。

这些请求头为：
>* <b>From</b> - 包含了用户的邮箱地址（如果有提供的话）
>* <b>User-Agent</b> - 包含了Web客户端的信息
>* <b>Referer</b> - 包含了用户的来源
>* <b>Authorization</b> - 包含了用户名和密码
>* <b>Client-ip</b> - 包含了用户的IP地址
>* <b>X-Forwarded-For</b> - 包含了用户的IP地址（当通过代理服务器来访问时）
>* <b>Cookie</b> - 包含了服务器生成的ID标签。

理论上，`From header`是标识用户的最理想选择，但在实际上，它却因为电子邮件收集的安全性问题而很少得到使用。

`user-agent header`包含了譬如浏览器版本，操作系统等信息。虽然这对自定义内容很重要，但它不会以次方式来识别用户。

`Referer header`告诉服务器用户是从哪里来的。这个信息常用于改善对于用户行为的认知，也不用来识别用户。

获取到上述的关于用户信息的请求头信息仍然还不够来有效地个性化定制用户内容。

剩下的请求头将能提供更精确的识别机制。

### IP address IP地址
在过去我们更多地用IP地址来识别用户，因为过去IP地址不容易进行伪装/交换。虽然它可以用于额外的安全检查，但它还不够可靠。

原因：
>* 它是用于描述机器的，不是用户
>* NAT防火墙　－　很多ISP（网络服务商）会使用NAT防火墙来加强安全性能并处理IP地址短缺。
>* 动态IP地址 - 用户经常从ISP那里获取动态IP地址。
>* HTTP代理和网关 - 它们可以隐藏原始IP地址。有一些代理服务器使用`Client-ip`或`X-Forwarded-For`来保留原始IP地址。

### Long(fat) URLs
网站利用网址来改善用户体验的方法并不罕见，当用户浏览网站时，他们会添加更多信息，直到网址看起来很复杂和难以辨认。

我们来看一个亚马逊商城的URL例子：
```c#
https://www.amazon.com/gp/product/1942788002/ref=s9u_psimh_gw_i2?ie=UTF8&fpl=fresh&pd_rd_i=1942788002&pd_rd_r=70BRSEN2K19345MWASF0&pd_rd_w=KpLza&pd_rd_wg=gTIeL&pf_rd_m=ATVPDKIKX0DER&pf_rd_s=&pf_rd_r=RWRKQXA6PBHQG52JTRW2&pf_rd_t=36701&pf_rd_p=1cf9d009-399c-49e1-901a-7b8786e59436&pf_rd_i=desktop
```

使用这种方法有几个缺点：
>* 不美观
>* 不可共享
>* 破环缓存
>* 仅适用于此节点
>* 增加了服务器的负载

### Cookies
现在最好最新的用户识别方法不包括验证，由Netscape开发，现在每个浏览器都支持它们。

Cookie类型有两种，一种为会话Cookies，一种为持久性Cookies。会话cookies在关闭浏览器时会被删除，持久性Cookies储存在硬盘里，可以保存更长时间。将会话Cookie转换为持久性Cookie需要设置Max-Age或Expiry属性。

现在的浏览器譬如Chrome和Firefox可以让你的后台进程在关闭时保持工作状态，以便你额可以恢复停止的位置。这样可能会导致会话Cookie的保存，使用时需要注意。

那么Cookies是如何工作的？

Cookies包含了一系列使用`Set-Cookie`或`Set-Cookie2`响应头的服务器集合的`name=value`值对列表。通常，存储在cookie中的信息是某种客户端ID，但是一些网站也存储其他信息。

浏览器将此信息存储在其Cookie数据库中，并在用户下次访问页面/网站时返回。 浏览器可以处理数千种不同的Cookie，并且知道何时为其服务。

下面我们来看一些例子：

1. User Agent -> Server
```c#
POST /acme/login HTTP/1.1
[form data]
```
用户通过表单输入识别自己

2. Server -> User Agent
```c#
HTTP/1.1 200 OK
Set-Cookie2: Customer="WILE_E_COYOTE"; Version="1"; Path="/acme"
```
服务器发送`Set-Cookie2`响应头，告诉用户代理（浏览器）设置Cookie中的用户信息。

3. User Agent -> Server
```c#
POST /acme/pickitem HTTP/1.1
Cookie: $Version="1"; Customer="WILE_E_COYOTE"; $Path="/acme"
[form data]
```
用户将商品加入购物车

4. Server -> User Agent
```c#
HTTP/1.1 200 OK
Set-Cookie2: Part_Number="Rocket_Launcher_0001"; Version="1"; Path="/acme"
```
购物车中含有一个商品

5. User Agent -> Server
```c#
POST /acme/shipping HTTP/1.1
Cookie: $Version="1"; Customer="WILE_E_COYOTE"; $Path="/acme"; 
        Part_Number="Rocket_Launcher_0001";
[form data]
```
用户选择运送方式。

6. Server -> User Agent
```c#
HTTP/1.1 200 OK
Set-Cookie2: Shipping="FedEx"; Version="1"; Path="/acme"
```
新的cookie反映了运送方式。

7. Usre Agent -> Server
```c#
POST /acme/process HTTP/1.1
Cookie: $Version="1";
        Customer="WILE_E_COYOTE"; $Path="/acme";
        Part_Number="Rocket_Launcher_0001"; $Path="/acme";
        Shipping="FedEx"; $Path="/acme"
[form data]
```

还有一件事你需要注意到的是，Cookies并不是完美的。除了安全性问题，还存在与REST架构风格相冲突的问题。 （有关滥用Cookie的部分）
