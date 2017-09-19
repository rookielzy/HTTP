# Udacity HTTP & Web Servers Courses

## Parts of a URI
`URI`可以说是一种资源名字，比如微博页面，维基百科的文章和一些像谷歌地图API的数据源都是`URI`。

例如： `https://en.wikipedia.org/wiki/Fish
这个例子中`URI`由三个不同的部分组成：

>* `https` 为URI协议名 `scheme`
>* `en.wikipedia.org` 为主机名 `hostname`
>* `/wiki/Fish` 为路径 `path`

### Scheme
`scheme`告诉客户端如何访问相关资源。我们常见的`scheme`有`http`, `https`, `file`。其中文件`URI`告诉客户端访问本地文件系统上的文件。HTTP和HTTPS `URI`指向Web服务器提供的资源。

[更多关于URI scheme]('http://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml')

### Hostname
主机名告诉客户端应该连接哪个服务器

我们经常可以看到网页地址只显示出主机名，但是在HTML中，我们不能写类似这样的代码：`<a href="www.google.com">this</a>`来去链接谷歌。主机名只能出现在支持它的`URI`协议名之后，例如HTTP，HTTPS。在这些`URI`中协议名个主机名之间总会有`://`跟着。

但是，并不是所有`URI`都有主机名。比如`mailto`只有一个邮箱地址：`mailto:spam@example.com`。这也说明了`URI`中`:`会跟在协议名后面，但是Mailto的链接没有主机名的部分，所以它不需要`//`。

### Path
路径，标识了服务器上的特定资源。一个服务器上可以有非常多的资源，路径名可以告诉服务器客户端正在找的具体是哪个资源。

### Relative URI references
在HTML中我们也经常看到类似这样的代码：`<a href="cliffsofinsanity.png">cliffsofinsanity.png</a>`。

像这种`URI`没有协议名和主机名，仅仅路径的`URI`我们叫做`relative URI reference.` 相对URI引用。它与它出现的上下文是“相对的”，具体来说就是它所在的页面。此URI不包括其所在服务器的主机名或端口，但浏览器可以从上下文中了解。如果我们点击其中一个链接，浏览器会从上下文中知道，它需要从与原始页面相同的服务器中获取它。