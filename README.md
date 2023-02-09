# 计算机网络结课作业

这是一份基于java socket编程的通信小程序，具有GUI图形化界面，同时利用线程池实现多线程通信。



![2022-12-10 (3)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-10 (3).png)

项目结构：

项目总体在org.example包下，其中MyServer为服务器，MyClient为客户端。ThreadSocket类为MyServer类中的核心线程，当MyServer服务器在某一端口开启监听时，每接收到客户端的一次连接请求时就创建一个线程，然后接着等待新的客户端连接。BroadcastListener类为MyClient客户端中的子线程，用于监听服务器发送的广播。



项目总体流程：

1.启动MyServer类，通过init方法创建一个客户端图形化界面

![2022-12-10 (5)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-10 (5).png)

2.输入指定端口(以10000端口为例)，点击start启动客户端，在命令行界面获得反馈，然后MyServer中```Socket clientSocket = serverSocket.accept();```方法开始等待客户端连接；

![2022-12-10 (7)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-10 (7).png)

3.启动服务器MyClient

![2022-12-10 (9)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-10 (9).png)

4.输入远程连接地址和端口号(以地址127.0.0.1，端口10000为例)，点击连接，创建新的GUI界面，并建立连接

```final Socket socket = new Socket(address, port);```

并开启一条新线程监听服务端广播

```java
//为客户端创建一个线程，用于接收服务端广播,并显示在textArea2上
Thread thread = new Thread(new BroadcastListener(textArea2));
thread.start();
```

于此同时服务端为该连接建立一个线程(ThreadSocket)，用于监听客户端之后将要发送的数据，服务器端将该连接对象放入HashSet集合中保存，用于之后广播

![2022-12-10 (11)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-10 (11).png)

5.在左侧文框中输入文本，点击发送按钮向客户端发送数据流

![2022-12-11 (3)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-11 (3).png)

6.客户端接收数据，并将该数据以广播形式发送给所有连接的客户端，客户端中用于接收广播的线程在循环中不断等待接收数据

![2022-12-11 (6)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-11 (6).png)

发送多条数据后

![2022-12-11 (8)](C:\Users\hp007\OneDrive\图片\屏幕快照\2022-12-11 (8).png)

7.关闭该页面，断开与服务器连接



总结：

socket(套接字)编程是TCP/IP协议在java应用中的一种实现，它将TCP/IP协议封装隐藏在socket接口中，在连接与断开连接的过程中帮我们实现了三次握手与四次挥手，使我们在编程过程中可以专注于逻辑的实现。

TCP是一种面向连接的、可靠的传输层协议，只有双方建立连接才能进行通信，这点与UDP(无连接、不可靠)正相反。

