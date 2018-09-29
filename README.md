# HTTP
http相关

# Nginx
## Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。
### 反向代理
什么是反向代理？ 互联网应用基本都基于CS基本结构，即client端和server端。代理其实就是在client端和真正的server端之前增加一层提供特定服务的服务器，即代理服务器。<br>
#### 1、正向代理
反向代理不好理解，正向代理大家总有用过，翻墙工具其实就是一个正向代理工具。它会把
们访问墙外服务器server的网页请求，代理到一个可以访问该网站的代理服务器proxy，这个代理服务器proxy把墙外服务器server上的网页内容获取，再转发给客户。具体的流程如下图<br>
![image](https://user-gold-cdn.xitu.io/2018/9/27/1661ac31c06b0681?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
