# HTTP
## http协议的原理

五层模型<br>
应用层、传输层、网络层、数据链路层、物理层<br>
物理层：定义物理设备如何传输数据，电脑的硬件，网卡，端口、网线<br>
数据链路层: 0101,<br>
网络层：数据在节点之间传输创建逻辑链路<br>
传输层：提供端到端的服务<br>
应用层：构建于TCP协议之上的<br>
### HTTP的三次握手<br>
客户端和服务器进行http请求和返回的过程中需要建立TCP connection这样一个连接，http在这基础上发送请求。
1、首先客户端向服务端发送我要创建一个连接的数据包的请求，SYN=1，Seq=X，SYN：标志位
2、服务端接收到数据包之后，知道客户端要和服务端创建连接，创建连接之后，服务端开启tcpsocket的一个端口，返回给客户端，返回的数据SYN=1，ACK=X+1，Seq=Y
3、客户端拿到之后，服务端允许连接，再发送ACK=Y+1，Seq=Z
为什么要进行三次握手？<br>
目的防止服务端开启一些无用的连接，因为网络传输是有延迟的，传输的过程中，服务端如果直接创建连接，服务端传输过程中，数据丢失了，客户端无法接收到服务端返回的数据，客户端设置一些请求超时的，客户端关闭连接创建，如果没有第三次握手在这里，服务端无法知道客户端接收没接收到返回的信息，并且没给出确认是开启请求还是关闭请求，那么服务端端口就一直开着等着客户端来发送实际的请求数据，那服务端的这次连接就浪费了。






# Nginx
## Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。
### 反向代理
什么是反向代理？ 互联网应用基本都基于CS基本结构，即client端和server端。代理其实就是在client端和真正的server端之前增加一层提供特定服务的服务器，即代理服务器。<br>
#### 1、正向代理
反向代理不好理解，正向代理大家总有用过，翻墙工具其实就是一个正向代理工具。它会把
们访问墙外服务器server的网页请求，代理到一个可以访问该网站的代理服务器proxy，这个代理服务器proxy把墙外服务器server上的网页内容获取，再转发给客户。具体的流程如下图<br>
![image](https://user-gold-cdn.xitu.io/2018/9/27/1661ac31c06b0681?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

概括说：就是客户端和代理服务器可以直接互相访问，属于一个LAN（局域网）；代理对用户是非透明的，即用户需要自己操作或者感知得到自己的请求被发送到代理服务器；代理服务器通过代理用户端的请求来向域外服务器请求响应内容。
#### 2、反向代理 反向代理则正好相反
在反向代理中（事实上，这种情况基本发生在所有的大型网站的页面请求中），客户端发送的请求，想要访问server服务器上的内容。但将被发送到一个代理服务器proxy，这个代理服务器将把请求代理到和自己属于同一个LAN下的内部服务器上，而用户真正想获得的内容就储存在这些内部服务器上。看到区别了吗，这里proxy服务器代理的并不是客户，而是服务器，即向外部客户端提供了一个统一的代理入口，客户端的请求，都先经过这个proxy服务器，至于在内网真正访问哪台服务器内容，由这个proxy去控制。一般代理是指代理客户端，而这里代理的对象是服务器，这就是“反向”这个词的意思。Nginx就是来充当这个proxy的作用。
概括说：就是代理服务器和真正server服务器可以直接互相访问，属于一个LAN（服务器内网）；代理对用户是透明的，即无感知。不论加不加这个反向代理，用户都是通过相同的请求进行的，且不需要任何额外的操作；代理服务器通过代理内部服务器接受域外客户端的请求，并将请求发送到对应的内部服务器上。<br>
#### 3、为什么要Nginx反向代理
使用反向代理最主要的两个原因：<br>
1）安全及权限。可以看出，使用反向代理后，用户端将无法直接通过请求访问真正的内容服务器，而必须首先通过Nginx。可以通过在Nginx层上将危险或者没有权限的请求内容过滤掉，从而保证了服务器的安全。<br>
2）负载均衡。例如一个网站的内容被部署在若干台服务器上，可以把这些机子看成一个集群，那么Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上（内部模块提供了多种负载均衡算法），从而实现服务器压力的负载均衡。此外，nginx还带有健康检查功能（服务器心跳检查），会定期轮询向集群里的所有服务器发送健康检查请求，来检查集群中是否有服务器处于异常状态，一旦发现某台服务器异常，那么在以后代理进来的客户端请求都不会被发送到该服务器上（直到后面的健康检查发现该服务器恢复正常），从而保证客户端访问的稳定性。<br>
