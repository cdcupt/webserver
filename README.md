# 简单的WEB服务器

---

这是一个简单的web服务器，能够处理一些简单的HTTP请求，目前在不断的完善中，更多功能敬请期待......
## 状态代码

**1XX：仅表示信息，没有1XX状态代码的定义，它们只保留给实验目的。**

**2XX：成功 其他的2xx状态码的其余部分主要是供脚本处理和不经常使用**。

“200” ; 服务端成功接收并处理了客户端的请求。

**3XX：重定向**

“301” ; 客户端所请求的URL已经移走，需要客户端重定向到其它的URL；

“304” ; 客户端所请求的URL未发生变化；

**4XX：客户端错误**

“400” ; 客户端请求错误；

“403” ; 客户端请求被服务端所禁止；

“404” ; 客户端所请求的URL在服务端不存在；

**5XX：服务器错误**

“500” ; 服务端在处理客户端请求时出现异常；

“501” ; 服务端未实现客户端请求的方法或内容；

“502” ; 此为中间代理返回给客户端的出错信息，表明服务端返回给代理时出错；

“503” ; 服务端由于负载过高或其它错误而无法正常响应客户端请求；

“504” ; 此为中间代理返回给客户端的出错信息，表明代理连接服务端出现超时。
## 使用说明
编译：

    $ make
运行

    $ ./web
通过本地回环127.0.0.1以及动态分配的端口号进行HTTP请求

## 2017.3.29:
一个简单的回射服务器，使用主线程统一的accept，未使用线程池
## 2017.3.30:
新增GET命令，可以获取简单的主页信息

改为使用线程池处理并发，在线程中调用accept
## 2017.4.6:
支持POST命令，支持CGI后端框架(参考tinyserver)

CGI支持：

使用两个管道，cgi_input 和 cgi_output;

 - 在子进程中，把 STDOUT 重定向到 cgi_outputt 的写入端，把 STDIN 重定向到 cgi_input 的读取端，关闭 cgi_input 的写入端 和 cgi_output 的读取端
 - 在父进程中，关闭 cgi_input 的读取端 和 cgi_output 的写入端，如果 POST 的话，把 POST 数据写入 cgi_input，已被重定向到 STDIN，读取 cgi_output 的管道输出到客户端，该管道输入是 STDOUT。

## 2017.4.9:
在池中增加epoll复用，ET模式需使用非阻塞I/O模型，将get_line改为非阻塞I/O，异步多线程的示例运行正常。参考半同步半异步模型，计划主线程处理I/O请求，业务逻辑分离给池内线程完成，但本项目涉及业务较为简单，故采用将epoll封装在池内线程内。

## 2017.4.25:
开始重构代码，c++风格

## 2017.6.21:
重新封装Epoll类，恢复到以前的c风格代码
