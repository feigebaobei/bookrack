# 三次握手四次分手

## http 连接

http协议是建立在tcp协议上的。  
http/1.0 每次请求要求建立一次单独的连接，处理完本次请求后，自动释放连接。  
http/1.1 可以在一次连接中处理多个请求，且多个请求可以重叠进行。（有些像异步）  

## socket

套接字（socket）是通信的基石，是tcp/ip协议的网络通信的基本操作单元。其中包括5种信息：  

- 连接使用的协议  
- 本地主机的ip.  
- 本地进程的协议端口。  
- 远地主机的ip  
- 远地主机的协议端口。  

建立socket连接至少需要一对套接字，一个运行在客户端：ClientSocket，另琴运行在服务器端：ServerSocket.
整个对接过程分为三年阶段：  
服务器监听：ServerSocket不指定具体的ClientSocket。它处于等待连接状态。实时监控网络状态，等待客户端的连接请求。  
客户端请求：ClientSocket提出连接请求。它首先描述要连接的ServerSocket，指出ServerSocket的地址和端口。然后向ServerSocket提出连接请求。  
确认连接：ServerSocket监听到ClientSocket的请求后，就响应ClientSocket的请求，建立一个新的线程，把ServerSocket的描述发给客户端。若ClientSocket确认了此描述，双方就正式建立连接。ServerSocket继续处于监听状态，继续接收其它ClientSocket的连接请求。

Scoket连接可以指定不同的传输层协议（tcp 或 udp）。若使用tcp协议进行连接时，该Socket连接就是一个TCP连接。

通常Socket连接使用要TCP协议。连接一旦建立，双方就可以传输数据。但是在实际网络中cs间有路由器，网关，防火墙等。大部分防火墙关闭长时间处于非活跃状态的Socket连接。因上需要轮询告诉网络该连接处于活跃状态。  
若Scoket连接使用http协议。则使用“请求——响应”的方式。只有在请求后才会响应。  

## tcp

TCP(Transmission Control Protocol 传输控制协议)是一种面向连接的、可靠的，基于字节流的传输层通信协议。  
tcp有7层。每一层都会在前一层上添加头部信息。  

    数据发送端       |         |         |         |         |   data  |
                    |         |         |         |   ah    |   data  |
                    |         |         |   ph    |   ah    |   data  |
                    |         |   sh    |   ph    |   ah    |   data  |
                    |   th    |   sh    |   ph    |   ah    |   data  |
                    |         |   sh    |   ph    |   ah    |   data  |
                    |         |         |   ph    |   ah    |   data  |
                    |         |         |         |   ah    |   data  |
    数据接收端       |         |         |         |         |   data  |

## tcp头部

