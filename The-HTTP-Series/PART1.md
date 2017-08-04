# THE HTTP SERIES(PART1): OVERVIEW OF THE BASIC CONCEPTS 基本概念

你将学习到如下知识点：
* What the HTTP is exactly
* Resources
* How the messages are exchanged between Web Client and Web Server
* Messages and some message examples
* MIME types
* Request Methods
* Headers
* Status codes

## HTTP definition HTTP的定义

[Tim Berners-Lee]('https://en.wikipedia.org/wiki/Tim_Berners-Lee')， `HTTP` 的创始人，同样也是万维网发明人之一。其中HTTP发展过程中有着重大贡献之一的人就是[Roy Fielding]('https://en.wikipedia.org/wiki/Roy_Fielding')，同时他也是　`REST` 架构风格的创始人。

<b>The Hypertext Transfer Protocol 超文本传输协议</b>是应用之间可以互相通信的一种协议。本质上来说， `HTTP` 负责客户端和服务器之间的所有互联网文件的收发，其中包括了HTML文件，图像文件，文本文件，多媒体文件以及它们之间的所有东西，同时它可以快速可靠地实现相应地操作。

`HTTP` 是应用协议，而不是传输协议，因为它被用于应用层中地通信。为了更直观地观察网络之间通信，我们可以参考下图：

<div align=center>
    <img src="img/Network-stack.png" />
</div>

从上图中，你可以清晰地看到 `HTTP` 确实作用于应用层，而 `TCP` 作用于传输层。

## Resources 资源

<div align=center>
    <img src="img/resource-1024x475.jpg" width="700" height="400" />
</div>

任何在互联网上的东西都叫做资源，同时 `HTTP` 与资源协同工作，其中包括了文件，流和服务等等。比如，一个HTML网页是一种资源，一段YouTube的视频也是一种资源，你的记录日常工作的网页应用也是一种资源。

那么，你又应该如何区分一种资源和另一种资源呢？

通过所给的 `URL` （统一资源定位符）

`URL` 指向浏览器能够找到相应资源的唯一位置。

## How the messages are exchanged between Web Client and Web Server 信息是如何在网页客户端和网页服务器之间交流的？

每一个内容，每一种资源都存在于一些Web服务器（HTTP服务器）上。这些服务器随时等待或处理着这些资源的HTTP请求。

但又如何从一个Web服务器上请求资源呢？

当然是需要一个HTTP客户端来完成上述工作。

在你正在阅读这篇文章时，你正在使用着HTTP客户端，也就是网页浏览器。它可以与HTTP服务器进行通信，然后将资源返回到你的计算机上。主流的浏览器有Google's Chrome, Mozilla's Firefox, Opera, Apple's Safari 和 臭名昭著的IE。

## Messages and some messages examples

那么HTTP消息是长什么样子的呢？

多说无益，这里有一些HTTP消息的例子：

### GET request GET请求

```http
GET /repos/CodeMazeBlog/ConsumeRestfulApisExamples HTTP/1.1
Host: api.github.com
Content-Type: application/json
Authorization: Basic dGhhbmtzIEhhcmFsZCBSb21iYXV0LCBtdWNoIGFwcHJlY2lhdGVk
Cache-Control: no-cache
```

### POST request POST请求

```http
POST /repos/CodeMazeBlog/ConsumeRestfulApisExamples/hooks?access_token=5643f4128a9cf974517346b2158d04c8aa7ad45f HTTP/1.1
Host: api.github.com
Content-Type: application/json
Cache-Control: no-cache
 
{
  "url": "http://www.example.com/example",
  "events": [
    "push"
  ],
  "name": "web",
  "active": true,
  "config": {
    "url": "http://www.example.com/example",
    "content_type": "json"
  }
}
```

上述分别为 `GET` 请求和 `POST` 请求的例子。让我们快速地浏览一下两者之间的不同。

第一行为保留的请求行。它由请求方法（`request method`）名字，请求统一资源标识符（`request URI`）和HTTP版本号（HTTP version）组成。

接下来的几代表请求头(`request headers`)。请求头为请求提供了额外的信息，比如请求期望的响应内容类型，验证信息等等。

对于 `GET` 请求，它就只包括上述内容。对于 `POST` 请求，它还可以拥有一个附加了额外信息的主体。在这个例子中，这是个一个JSON消息，其中包含了应该如何为`URI`中给定的`repo`创建`GitHub webhook`的附加信息。该消息是`webhook`创建所必需的，因此我们要使用`POST`请求将该信息提供给`GitHub API`。

请求行和请求标头后面必须跟<CR><LF>（回车符和换行符\r\n），消息标题和仅包含CRLF的消息正文之间只有一行空行。

HTTP请求参考：[https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html]('https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html')

然后我们又应该如何处理这些请求呢？

### Response message 响应消息

```http
HTTP/1.1 200 OK
Server: GitHub.com
Date: Sun, 18 Jun 2017 13:10:41 GMT
Content-Type: application/json; charset=utf-8
Transfer-Encoding: chunked
Status: 200 OK
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4996
X-RateLimit-Reset: 1497792723
Cache-Control: private, max-age=60, s-maxage=60
 
[
  {
    "type": "Repository",
    "id": 14437404,
    "name": "web",
    "active": true,
    "events": [
      "push"
    ],
    "config": {
      "content_type": "json",
      "insecure_ssl": "0",
      "url": "http://www.example.com/example"
    },
    "updated_at": "2017-06-18T12:17:15Z",
    "created_at": "2017-06-18T12:03:15Z",
    "url": "https://api.github.com/repos/CodeMazeBlog/ConsumeRestfulApisExamples/hooks/14437404",
    "test_url": "https://api.github.com/repos/CodeMazeBlog/ConsumeRestfulApisExamples/hooks/14437404/test",
    "ping_url": "https://api.github.com/repos/CodeMazeBlog/ConsumeRestfulApisExamples/hooks/14437404/pings",
    "last_response": {
      "code": 422,
      "status": "misconfigured",
      "message": "Invalid HTTP Response: 404"
    }
  },
]
```

响应消息的结构与请求基本相同，除了被称为状态行（`response status`）的第一行，状态行附带有关响应状态的信息。

状态行后面跟着响应头（`response header`）和响应体（`response body`）。

HTTP响应参考： [https://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html]('https://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html')

## MIME types MIME类型

MIME类型是一种描述互联网上扽文件类型的标准。你的浏览器有着一个MIME类型的列表，同样适用于Web服务器。通过这种方式，文件可以顺利收发无论在什么操作系统上。

有趣的是MIME实际上表示为多用途互联网邮件扩展（Multipurpose Internet Mail Extension），因为一开始MIME是被设计为多媒体邮件所使用的。后来它被适配与HTTP协议和其他不同的协议。

任何MIME类型由`type`类型，`subtype`子类型和一系列的`optional parameters`可选参数组成，并遵循如下格式：`type/subtype; optional parameters`。

下面是一些简单的例子：

```http
Content-Type: application/json
Content-Type: text/xml; charset=utf-8
Accept: image/gif
```

你可以在如下链接中找到一些常用的MIME类型和子类型。　[HTTP reference]('https://www.code-maze.com/the-http-reference/#mimetypes')

## Request Methods 请求方法

HTTP的请求方法（也可以把它看作动词）定义将对资源执行的操作。[HTTP定义了不同的请求方法]('http://www.code-maze.com/the-http-reference#requestmethods')，其中最常用的为`GET`请求和`POST`请求。

请求方法可以是幂等的也可以是非幂等的。这只是一种方便的解释在同一个资源上多次请求是否安全的术语。换句话说，就是`GET`请求唯一的目的就是检索信息，所以默认情况下是幂等的。一遍又一遍地在同一资源上调用`GET`请求不应该产生不同的响应。另外，`POST`请求是非幂等的。

`HTTP/1.1`之前就只有三种请求方法：`GET`,`POST`和`HEAD`。`HTTP/1.1`时期多了如下几种请求方法：`OPTIONS`,`PUT`,`DELETE`,`TRACE`和`CONNECT`。

想要查看每种请求方法是干什么的可以参考：[HTTP reference]('https://www.code-maze.com/the-http-reference#requestmethods')

