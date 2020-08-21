# 计算机网络知识

## 计算机网络知识汇总
### 1、TCP为什么需要3次握手，4次断开？

“三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。
主要目的防止server端一直等待，浪费资源。
为什么4次断开？

在tcp连接握手时为何ACK是和SYN一起发送，这里ACK却没有和FIN一起发送呢。原因是因为tcp是全双工模式，接收到FIN时意味将没有数据再发来，但是还是可以继续发送数据。

###  2、TCP和UDP有什么区别？
TCP是传输控制协议，提供的是面向连接、可靠的字节流服务。通信双方彼此交换数据前，必须先通过三次握手协议建立连接，之后才能传输数据。TCP提供超时重传，丢弃重复数据，检验数据，流量控制等功能，保证数据能从一端传到另一端。UDP是用户数据报协议，是一个简单的面向无连接的协议。UDP不提供可靠的服务。在数据数据前不用建立连接故而传输速度很快。UDP主要用户流媒体传输，IP电话等对数据可靠性要求不是很高的场合。<br>

### 3、TCP拥塞控制？
几种拥塞控制方法
慢开始和拥塞避免
快速重传
快速恢复

### 4、在浏览器中输入www.baidu.com后执行的全部过程。
答：1、客户端浏览器通过DNS解析到www.baidu.com的IP地址220.181.27.48，通过这个IP地址找到客户端到服务器的路径。客户端浏览器发起一个HTTP会话到220.161.27.48，然后通过TCP进行封装数据包，输入到网络层。

　　2、在客户端的传输层，把HTTP会话请求分成报文段，添加源和目的端口，如服务器使用80端口监听客户端的请求，客户端由系统随机选择一个端口如5000，与服务器进行交换，服务器把相应的请求返回给客户端的5000端口。然后使用IP层的IP地址查找目的端。
　　
　　3、客户端的网络层不用关心应用层或者传输层的东西，主要做的是通过查找路由表确定如何到达服务器，期间可能经过多个路由器，这些都是由路由器来完成的工作，我不作过多的描述，无非就是通过查找路由表决定通过那个路径到达服务器。
　　
　　4、客户端的链路层，包通过链路层发送到路由器，通过邻居协议查找给定IP地址的MAC地址，然后发送ARP请求查找目的地址，如果得到回应后就可以使用ARP的请求应答交换的IP数据包现在就可以传输了，然后发送IP数据包到达服务器的地址。
　　
### http请求code码代表含义
      201-206都表示服务器成功处理了请求的状态代码，说明网页可以正常访问。
        200（成功）  服务器已成功处理了请求。通常，这表示服务器提供了请求的网页。
        201（已创建）  请求成功且服务器已创建了新的资源。
        202（已接受）  服务器已接受了请求，但尚未对其进行处理。
        203（非授权信息）  服务器已成功处理了请求，但返回了可能来自另一来源的信息。
        204（无内容）  服务器成功处理了请求，但未返回任何内容。
        205（重置内容） 服务器成功处理了请求，但未返回任何内容。与 204 响应不同，此响应要求请求者重置文档视图（例如清除表单内容以输入新内容）。
        206（部分内容）  服务器成功处理了部分 GET 请求。
        
        300-3007表示的意思是：要完成请求，您需要进一步进行操作。通常，这些状态代码是永远重定向的。

        300（多种选择）  服务器根据请求可执行多种操作。服务器可根据请求者 来选择一项操作，或提供操作列表供其选择。

        301（永久移动）  请求的网页已被永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置。您应使用此代码通知搜索引擎蜘蛛网页或网站已被永久移动到新位置。
        302（临时移动） 服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到不同的位置。但由于搜索引擎会继续抓取原有位置并将其编入索引，因此您不应使用此代码来告诉搜索引擎页面或网站已被移动。
        303（查看其他位置） 当请求者应对不同的位置进行单独的 GET 请求以检索响应时，服务器会返回此代码。对于除 HEAD 请求之外的所有请求，服务器会自动转到其他位置。
        304（未修改） 自从上次请求后，请求的网页未被修改过。服务器返回此响应时，不会返回网页内容。

        如果网页自请求者上次请求后再也没有更改过，您应当将服务器配置为返回此响应。由于服务器可以告诉 搜索引擎自从上次抓取后网页没有更改过，因此可节省带宽和开销。

        305（使用代理） 请求者只能使用代理访问请求的网页。如果服务器返回此响应，那么，服务器还会指明请求者应当使用的代理。
        307（临时重定向）  服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到不同的位置。但由于搜索引擎会继续抓取原有位置并将其编入索引，因此您不应使用此代码来告诉搜索引擎某个页面或网站已被移动。

        4XXHTTP状态码表示请求可能出错，会妨碍服务器的处理。

        400（错误请求） 服务器不理解请求的语法。

        401（身份验证错误） 此页要求授权。您可能不希望将此网页纳入索引。

        403（禁止） 服务器拒绝请求。

        404（未找到） 服务器找不到请求的网页。例如，对于服务器上不存在的网页经常会返回此代码。

        例如：http://www.0631abc.com/20100aaaa，就会进入404错误页面

        405（方法禁用） 禁用请求中指定的方法。

        406（不接受） 无法使用请求的内容特性响应请求的网页。

        407（需要代理授权） 此状态码与 401 类似，但指定请求者必须授权使用代理。如果服务器返回此响应，还表示请求者应当使用代理。

        408（请求超时） 服务器等候请求时发生超时。

        409（冲突） 服务器在完成请求时发生冲突。服务器必须在响应中包含有关冲突的信息。服务器在响应与前一个请求相冲突的 PUT 请求时可能会返回此代码，以及两个请求的差异列表。

        410（已删除） 请求的资源永久删除后，服务器返回此响应。该代码与 404（未找到）代码相似，但在资源以前存在而现在不存在的情况下，有时会用来替代 404 代码。如果资源已永久删除，您应当使用 301 指定资源的新位置。

        411（需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。

        412（未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。

        413（请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。

        414（请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。

        415（不支持的媒体类型） 请求的格式不受请求页面的支持。

        416（请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态码。

        417（未满足期望值） 服务器未满足"期望"请求标头字段的要求。

500至505表示的意思是：服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。

        500（服务器内部错误）  服务器遇到错误，无法完成请求。

        501（尚未实施） 服务器不具备完成请求的功能。例如，当服务器无法识别请求方法时，服务器可能会返回此代码。

        502（错误网关） 服务器作为网关或代理，从上游服务器收到了无效的响应。

        503（服务不可用） 目前无法使用服务器（由于超载或进行停机维护）。通常，这只是一种暂时的状态。

        504（网关超时）  服务器作为网关或代理，未及时从上游服务器接收请求。

        505（HTTP 版本不受支持） 服务器不支持请求中所使用的 HTTP 协议版本。


## 三次握手，四次挥手相关知识
### 5、什么是SYN攻击？
答：
在三次握手过程中，服务器发送 SYN-ACK 之后，收到客户端的 ACK 之前的 TCP 连接称为半连接(half-open connect)。此时服务器处于 SYN_RCVD 状态。当收到 ACK 后，服务器才能转入 ESTABLISHED 状态.

SYN攻击指的是，攻击客户端在短时间内伪造大量不存在的IP地址，向服务器不断地发送SYN包，服务器回复确认包，并等待客户的确认。由于源地址是不存在的，服务器需要不断的重发直至超时，这些伪造的SYN包将长时间占用未连接队列，正常的SYN请求被丢弃，导致目标系统运行缓慢，严重者会引起网络堵塞甚至系统瘫痪。

SYN 攻击是一种典型的 DoS/DDoS 攻击。

检测 SYN 攻击非常的方便，当你在服务器上看到大量的半连接状态时，特别是源IP地址是随机的，基本上可以断定这是一次SYN攻击。在Linux/Unix 上可以使用系统自带的 netstats 命令来检测 SYN 攻击。

SYN攻击不能完全被阻止，除非将TCP协议重新设计。我们所做的是尽可能的减轻SYN攻击的危害，常见的防御 SYN 攻击的方法有如下几种：

缩短超时（SYN Timeout）时间

增加最大半连接数

过滤网关防护

SYN cookies技术

### 6、Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？
答：

1）Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。

2）它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。
### 7、Mybatis都有哪些Executor执行器？它们之间的区别是什么？
答：Mybatis有三种基本的Executor执行器，SimpleExecutor、ReuseExecutor、BatchExecutor。

1）SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

2）ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map

3）BatchExecutor：完成批处理。