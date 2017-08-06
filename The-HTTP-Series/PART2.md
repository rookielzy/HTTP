# THE HTTP SERIES(PART 2): ARCHITECTURAL ASPECTS

在PART 1中，我们讨论了HTTP的基础概念，接下来我们将基于PART 1的内容更进一步地进行探讨，我们将探讨HTTP架构方面的东西而不只是数据的收发。

HTTP作为应用协议无法自我发挥作用，它需要基于硬件和软件的解决方案来提供不同的服务，并使互联网通信称为可能并且高校。

在本章节，你将学习到如下知识点：

* Web Servers
* Proxy Servers
* Caching
* Gateways, Tunnels, and Relays
* Web Crawlers

这些是互联网的组成部分，你将了解到它的每一部分是干什么的，还有怎么进行工作的。这些知识将帮助你把PART 1里的知识点连成线，从而更好地了解HTTP通信的流程。

## Web Servers Web服务器

正如PART 1所阐述的那样，Web服务器最基本的功能就是储存[资源]('http://www.code-maze.com/http-protocol-overview-part1/#Resources')和在接受到请求后为它们服务。你可以通过Web客户端（即浏览器）访问Web服务器，并返回获取所请求的资源或更改现有资源的状态。Web服务器也可以通过网页爬虫自动获取相关信息，我们将在本文后面讲到。

<div align="center">
    <img src="img/servers.jpg">
</div>

你或许也听说过一些比较流行的Web服务器，譬如`Apache HTTP Server`，`Nginx`，`IIS`和`Glassfish`等等。

软件应用中的Web服务器可以有简单到复杂，现代Web服务器可以执行很多不同的任务。Web服务器应该能够执行如下的基本任务：

* <b>设置连接</b> - 接受或关闭客户端连接
* <b>接受请求</b> - 读取HTTP请求消息
* <b>处理请求</b> - 解析请求消息并处理
* <b>获取资源</b> - 获取消息中指定的资源
* <b>构建响应</b> - 创建HTTP响应消息
* <b>发送响应</b> - 发送响应消息给客户端
* <b>日志记录</b> - 将完整的请求响应过程记录在日志里

接下来我将会把Web服务器的工作流程拆成几个阶段来讲，每个阶段都代表着Web服务器工作流程简化步骤。