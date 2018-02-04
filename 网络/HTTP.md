[TOC]

# HTTP概述

HTTP(HyperText Transport Protocol)——超文本传输协议，属于应用层协议，使用TCP/IP协议传输。

主要包含以下版本：

- HTTP/0.9，只支持GET方法，不支持多媒体内容的MIME类型、HTTP首部、版本号等，初衷是获取简单的HTML内容；
- HTTP/1.0，被广泛使用，增加了首部、MIME、额外的方法等；
- HTTP/1.0+，增加了keep-alive、虚拟主机支持等特性；
- HTTP/1.1，校正HTTP设计缺陷、明确语义、优化性能等。






# URL与资源

因特网资源使用URI来表示，具体如下：

- URI(Uniform Resource Identifier)——统一资源标识符，有两种形式，URL和URN。
- URN(Uniform Resource Name)——统一资源名，作为特定内容的唯一名称使用，与资源所在位置无关。
- URL(Uniform Resource Locator)——统一资源定位符，描述了服务器某个特定资源的位置。

## URL

URL通常包含以下三部分：

- scheme，及访问资源的协议类型，例如http://，htp://等；

- 主机名（IP地址），可通过域名服务（Domain Name Service，DNS）将主机名转换为IP地址，例如www.baidu.com；
- 资源路径，例如/images/first.png。

URL的通用语法为：

> <scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>

最重要的三部分即<scheme>、<host>、<path>。

- scheme：采用哪种协议访问资源，http://、htp://；
- user：一些scheme访问资源时需要使用用户名；
- password：用户名后面可能要包含的密码，`:`分割；
- host：资源宿主服务器的主机名或IP地址；
- port：资源宿主服务器正在监听的端口号，HTTP默认为80，HTTPS默认为443；
- path：资源在服务器位置，使用`/`与前面内容分割；
- params：参数为name/value对，可包含多个参数，多个参数使用`；`分割；
- query：查询使用`？`与之前内容分割，多个查询之间使用`&`分割；






# HTTP报文

HTTP报文是在HTTP应用程序之间发送的数据块。报文在客户端、服务器和代理之间流动，HTTP 报文会像河水一样流动。不管是请求报文还是响应报文，所有报文都会向下游(downstream)流动。所有报文的发送者都在接收者的上游 (upstream)。如下图，

<img src="https://raw.githubusercontent.com/xiaoaihhh/bnotes/master/images/http/HTTP报文流动方向.png" width="100%" height="50%" align=center>

## 报文的组成部分

报文由三个部分组成:对报文进行描述的**起始行(start line)**、包含属性的**首部(header)**块，以及可选的、包含 数据的**主体(body)**部分。起始行与首部由第一个回车符分割，首部与主体由第一个换行符分割。所有的 HTTP 报文都可以分为两类:请求报文(request message)和响应报文 (response message)。请求报文会向 Web 服务器请求一个动作。响应报文会将请求的结果返回给客户端。请求和响应报文的基本报文结构相同。

请求报文的格式:

```
<method> <request-URL> <version>
<headers>

<entity-body>
```

响应报文的格式(只有起始行的语法有所不同):

```
<version> <status-code> <reason-phrase> 
<headers>

<entity-body>
```

### 起始行

#### 方法<method>

客户端希望服务器对资源执行的动作。是一个单独的词，比如 GET、HEAD 或 POST。

HTTP常用方法如下表：

| 方法      | 功能                        | 是否包含主体 |
| :------ | :------------------------ | ------ |
| GET     | 从服务器获取一份文档                | 否      |
| HEAD    | 只从服务器获取文档的首部              | 否      |
| POST    | 向服务器发送需要处理的数据             | 是      |
| PUT     | 将请求的主体部分存储在服务器上           | 是      |
| TRACE   | 对可能经过代理服务器传送到服务器上去的报文进行追踪 | 否      |
| OPTIONS | 决定可以在服务器上执行哪些方法           | 否      |
| DELETE  | 从服务器上删除一份文档               | 否      |

#### 请求URL<request-URL>

命名了所请求资源，或者 URL 路径组件的完整 URL。如果直接与服务器进行对 话，只要 URL 的路径组件是资源的绝对路径。

#### 版本<version>

报文所使用的 HTTP 版本，其格式为:`HTTP/x.y`。在比较 HTTP 版本时，每个数字都必须单独进行比较，以便确定哪个版本更高。比如，HTTP/2.22 就比 HTTP/2.3 的版本要高，因为 22 比 3 大。

#### 状态码<status-code>

三位数字描述了请求过程中所发生的情况。每个状态码的第一位数字都用于描 述状态的一般类别（“成功”、“出错”等）。状态码分类如下：

| 范围      | 已定义范围   | 分类                                       |
| ------- | ------- | ---------------------------------------- |
| 100~199 | 100~101 | 信息性状态码，由于信息提示                            |
| 200~299 | 200~206 | 成功状态码                                    |
| 300~399 | 300~305 | 重定向状态码。重定向状态码要么告知客户端使用替代位置来访问他们所感兴趣的资源，要么就提 供一个替代的响应而不是资源的内容。如果资源已被移动，可发送一个重定向状态 码和一个可选的 Location 首部来告知客户端资源已被移走，以及现在可以在哪里 找到它 |
| 400~499 | 400~415 | 客户端错误状态码                                 |
| 500~599 | 500~505 | 服务器错误状态码                                 |

#### 原因短语<reason-phrase>

数字状态码的可读版本，例如HTTP/1.0 200 OK 。



### 首部

HTTP首部可以分为以下几类：

- 通用首部，既可以出现在请求报文中，也可以出现在响应报文中；
- 请求首部，提供更多有关请求的信息；
- 响应首部，提供更多有关响应的信息；
- 实体首部，描述主体的长度和内容，或者资源自身；
- 扩展首部，规范中没有定义的新首部。

HTTP首部都有一种简单的语法:名字后面跟着冒号( :)，然后跟上可选的空格，再跟上字段值，最后是一个空行。长的首部行可分为多行，以提高可读性，多出来的每行前面至少要有一个空格或制表符(tab)。例如

```
HTTP/1.0 200 OK 
Content-Type: image/gif 
Content-Length: 8572
Server: Test Server 
	Version 1.0
```

其中Server首部的值被划分成了多个延续行，其完整值为Test Server Version 1.0。

### 主体

HTTP报文的第三部分是可选的实体主体部分。实体的主体是HTTP报文的负荷，就是HTTP要传输的内容。