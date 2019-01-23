## Java 基础加强

### 网络编程

#### InetAddress

> JDK中提供了一个InetAddress类，用于封装一个IP地址，并提供了一系列与IP地址相关的方法

| 方法声明                           | 功能描述                                                     |
| :--------------------------------- | ------------------------------------------------------------ |
| InetAddress getByName(String host) | 参数host表示指定的主机，该方法用于在给定主机名的情况下确定主机的IP地址 |
| InetAddress getLocalHost()         | 创建一个表示本地主机的InetAddress对象                        |
| String getHostName()               | 得到IP地址的主机名，如果是本地主机则是计算机名，不是本机则是主机名，如果没有域名则是IP地址 |
| boolean isReachable(int timeout)   | 判断指定的时间内地址是否可以到达                             |
| String getHostAddress()            | 得到字符串格式的原始IP地址                                   |

#### UDP与TCP协议

* UDP协议是无连接协议，即在数据传输时，数据的发送端与接收端不建立逻辑连接
* TCP协议是面向连接的通信协议，即在传输数据前先在发送端和接收端建立逻辑连接，然后再进行数据传输。
* TCP连接中，必须明确客户端与服务端，由客户端发出连接请求，每次连接都要经过***“三次握手”***
  * 第一次：由客户端向服务器端发出连接请求，等待服务器确认
  * 第二次：服务器端向客户端回送一个响应，通知客户端收到连接请求
  * 第三次：客户端再次向服务器端发送确认信息，确认连接。

![](C:\Users\18451\Desktop\note\Java笔记\images\TCP三次握手.png)

#### UDP 通信

##### DatagramPacket

> DatagramPacket类是由JDK提供，该类的实例对象将相当于一个“集装箱”，用于封装UDP通信中发送或者就收的数据

**DatagramPacket  构造方法**

* DatagramPacket(byte[] buf, int length)

  使用该构造方法在创建DatagramPacket对象时，指定了封装数据的字节数组和数据的大小，没有指定IP地址和端口号。这样的方式只适用于**接收端**。

* DatagramPacket(byte [] buf, int length, InetAddress addr, int port)

  指定了字节数组、数据的大小、数据包的目标IP地址(addr)和端口号(port)，该方法通常用于**接收端**

* DatagramPacket(byte [] buf, int offset, int length)

  该方法只适用于**接收端**，增加了offset参数，该参数用于指定接收到的数据在存入buf缓冲数组时是从offset开始的。

* DatagramPacket(byte [] buf, int offset, int length, InetAddress addr, int port)

  offset参数用于指定一个数组中发送数据的偏移量为offset，即从offset位置开始发送数据，该方法用于**接收端**

  

  ​							DatagramPacket类中的常用方法

  | 方法声明                 | 功能描述                                                     |
  | ------------------------ | ------------------------------------------------------------ |
  | InetAddress getAddress() | 该方法用于返回发送端或者接收端的IP地址，如果是发送端的DatagramPacket对象，就返回接收端的IP地址，反之，就返回发送端的IP地址 |
  | int getPort()            | 该方法用于返回发送端或者接收端的端口号，如果是发送端的DatagramPacket对象，就返回接收端的端口号，反之，就返回发送端的端口号 |
  | byte[] getData()         | 该方法用于返回将要接收或者将要发送的数据，如果是发送端的DatagramPacket对象，就返回将要发送的数据，反之，就返回接收到的数据 |
  | int getLength()          | 该方法用于返回接收或者将要发送的数据的长度，如果是发送端的DatagramPacket对象，就返回将要发送的数据长度，反之，就返回接收到的数据的长度 |

  

  

##### DatagramSocket

> DatagramSocket类的作用就类似于码头，使用这个类的实例对象就可以发送或者接收DatagramSocket数据包。

* DatagramSocket()

  该方法用于创建发送端的DatagramSocket对象，在创建DatagramSocket对象时，并没有指定端口号，此时，系统会自动分配一个没有被其他网络程序占用的端口号

* DatagramSocket(int port)

  该构造方法可用于创建接收端的DatagramSocket对象，又可以创建发送端的DatagramSocket对象，在创建接收端DatagramSocket对象时，必须指定一个端口号，用于监听指定的端口

* DatagramSocket(int port, InetAddress addr)

  即指定端口号，也指定IP地址，适用于多块网卡的情况

| 方法声明                       | 功能描述                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| void receive(DatagramPacket p) | 该方法用于将接收到的数据填充到DatagramPacket数据包中，在接收到数据之前会一直处于阻塞状态，只有当接收到数据包时，该方法才会返回 |
| void send(DatagramPacket p)    | 该方法用于发送DatagramPacket数据包，发送的数据包中包含将要发送的数据、数据的长度、远程主机的IP地址和端口号 |
| void close()                   | 关闭当前的Socket，通知驱动程序释放为这个Socket保留的资源     |

#### TCP通信

##### ServerSocket

> JDK 提供了一个ServerSocket类，该类的实例对象用于实现一个服务器端的程序

* ServerSocket()

  该构造方法在创建ServerSocket对象时，并没有绑定端口号，这样的对象创建的服务器没有监听任何端口，不能直接使用，还需要继续调用bind(SocketAddress endpoint)方法将其绑定到指定的端口号上，才可以正常使用。

* ServerSocket(int port)

  该构造方法在创建ServerSocket对象时，就可以将其绑定到一个指定的端口号上（参数port就是端口号）。端口号可以指定为0，此时系统会分配一个还没有被其他网络程序所使用的端口号。由于客户端需要根据指定的端口号来访问服务器端程序，因此端口号随机分配的情况并不常用，通常都会让服务器端程序监听一个指定的端口号

* ServerSocket(int port, int backlog)

  backlog参数用于指定在服务器繁忙时，可以与之保持连接请求的等待客户数量，如果没有指定这个参数，默认为50

* ServerSocket(int port, int backlog, InetAddress bindAddr)

  适用于多块网卡的情况，bindAddr用于指定哪块网卡或IP地址上等待客户的连接请求

  ​								ServerSocket的常用方法

| 方法声明                          | 功能描述                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| Socket accept()                   | 该方法用于等待客户端的连接，在客户端连接之前一直处于阻塞状态，如果有客户端连接就会返回一个与之对应的Socket对象 |
| InetAddress getInetAddress()      | 该方法用于返回一个InetAddress对象，该对象中封装了ServerSocket绑定的IP地址 |
| boolean isClosed()                | 该方法用于判断ServerSocket对象是否为关闭状态，如果是关闭状态则返回true，反之则返回false |
| void bind(SocketAddress endpoint) | 该方法用于将ServerSocket对象绑定到指定的IP地址和端口号，其中参数endpoint封装了IP地址和端口号 |

##### Socket

> JDK 提供了一个Socket类，用于实现TCP的客户端程序

* Socket()

  该构造方法在创建Socket时，并没有制定IP地址和端口号，也就意味着只创建了客户端对象，并没有去连接任何服务器。通过该构造方法创建对象后还需调用connect(SocketAddress endpoint)方法，才能完成与指定服务器的连接，其中的参数endpoint用于封装IP地址和端口号

* Socket(String host, int port)

  该构造方法在创建Socket时，会根据参数去连接在指定的地址的端口上运行的服务器程序，其中参数host接收的是一个字符串类型的IP地址

* Socket(InetAddress address, int port)

  该方法与第二个构造方法类似，参数address用于接收一个InetAddress类型的对象，该对象用于封装一个IP地址。

| 方法声明                       | 功能描述                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| int getPort()                  | 返回Socket对象与服务器端连接的端口号                         |
| InetAddress getLocalAddress()  | 获取Socket对象绑定的本地IP地址，并将IP地址封装成InetAddress类型的对象返回 |
| void close()                   | 关闭Socket连接，结束本次通信                                 |
| InputStream getInputStream()   | 返回一个InputStream类型的输入流对象，如果该对象是由服务器端Socket返回，就用于读取客户端发送的数据，反之，用于读取服务器端发送的数据 |
| OutputStream getOutputStream() | 返回一个OutputStream类型的输出流对象，如果该对象是由服务器端Socket返回，就用于向客户端发送数据，反之，用于向服务器端发送数据 |

![](C:\Users\18451\Desktop\note\Java笔记\images\TCP客户端与服务器端通信图.png)