一. session和cookie区别?
    
    1.存储位置:cookie存储在客户端,session一般存储在服务器或数据库中;
    2.安全性: cookie因为存在本地浏览器安全性较弱于session
    3.生命周期:cookie的失效时间是累计的,session是间隔的
    4.网络传输:cookie可以通过网络实现客户端和服务器端的传输，而session存储在服务器端，无需传输。
    注意:cookie生成是在服务器端生成的,服务器端向客户端发送Cookie是通过HTTP响应报文实现的;
![cookie](https://github.com/goinghacker/images/blob/master/2.jpg)  

 
二. 禁用cookie后session还能使用吗？

    可以,HTTP协议是无状态的，想要跟踪客户的状态，会话机制来解决。
    默认情况下是基于cookie的，客户端请求服务器端生成sessionid，返回给客户端也就是cookie，
    但当浏览器禁用cookie后，可以通过get的形式将sessionid返回给浏览器端去接收
    
三. session如何共享以及区别？
    
    1、不使用session，换用cookie
    session是存放在服务器端的，cookie是存放在客户端的，我们可以把用户访问页面产生的session放到cookie里面，就是以cookie为中转站。
    你访问web服务器A，产生了session然后把它放到cookie里面，当你的请求被分配到B服务器时，服务器B先判断服务器有没有这个session，
    如果没有，再去看看客户端的cookie里面有没有这个session，如果也没有，说明session真的不存，如果cookie里面有，
    就把cookie里面的sessoin同步到服务器B，这样就可以实现session的同步了。
    说明：这种方法实现起来简单，方便，也不会加大数据库的负担，但是如果客户端把cookie禁掉了的话，那么session就无从同步了，
    这样会给网站带来损失；cookie的安全性不高，虽然它已经加了密，但是还是可以伪造的。
    2、session存在数据库（MySQL等）中
    PHP可以配置将session保存在数据库中，这种方法是把存放session的表和其他数据库表放在一起，如果mysql也做了集群了话，
    每个mysql节点都要有这张表，并且这张session表的数据表要实时同步。
    说明：用数据库来同步session，会加大数据库的IO，增加数据库的负担。而且数据库读写速度较慢，不利于session的适时同步。
    3、session存在memcache或者redis中
    memcache可以做分布式，php配置文件中设置存储方式为memcache，这样php自己会建立一个session集群，将session数据存储在memcache中。
