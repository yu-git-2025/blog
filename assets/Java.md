# Java

封装

继承

多态

抽象

接口

Lambda表达式

匿名内部类

泛型

网络编程,即时通信

- 基本的通信架构有2种形式:CS架构(Client客户端/Server服务端)、BS架构(Browser浏览器/Server服务端)

<img src="D:\桌面\CS架构.png" style="zoom: 25%;" />

<img src="D:\桌面\BS架构.png" style="zoom:25%;" />

- 网络通信三要素
  1. IP地址[设备在网络中的地址,是设备在网络中的唯一标识]
  2. 端口[应用程序在设备中的唯一标识]
  3. 协议[连接和数据在网络中传输的规则]



###### 客户端开发,UDP通信协议

```java
package com.Yu.UDP;
//客户端开发,UDP通信协议
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.util.Scanner;

public class UDPClient {
    public static void main(String[] args) throws Exception {
        System.out.println("-------客户端启动-------");
        DatagramSocket socket=new DatagramSocket(); //随机端口号

        Scanner sc=new Scanner(System.in);
        while(true){
            System.out.println("请输入要发送的内容：");
            String str=sc.nextLine();
            if("exit".equals(str)){
                System.out.println("-------客户端退出-------");
                socket.close();
                break;
            }

            byte[] bytes=str.getBytes();
            DatagramPacket packet=new DatagramPacket(bytes,bytes.length, 												InetAddress.getLocalHost(),8080);
            //发送数据包
            socket.send(packet);
        }
    }
}

```

###### 服务端开发,UDP通信协议

```java
package com.Yu.UDP;
//服务端开发,UDP通信协议
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPServer {
    public static void main(String[] args) throws Exception{
        System.out.println("-------服务端启动-------");
        DatagramSocket socket=new DatagramSocket(8000);

        byte[] bytes=new byte[1024*64]; //最大64KB
        DatagramPacket packet=new DatagramPacket(bytes,bytes.length);

        while(true){
            socket.receive(packet); //等待接收数据
            int len=packet.getLength();
            String data=new String(bytes,0,len);
            System.out.println("服务端收到数据："+data);

            //获取对方IP对象和程序端口
            String ip=packet.getAddress().getHostAddress();
            int port=packet.getPort();
            System.out.println("客户端IP:"+ip+",端口:"+port);
        }
    }
}
```

