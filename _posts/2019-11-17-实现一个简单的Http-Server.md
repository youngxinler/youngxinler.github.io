---
title: 实现一个简单的Http-Server
tags: Java
---

### 极致简单

通过ServerSock代理一个端口号, 在接收到请求的时候, 建立一个Sock实例(TCP连接),  然后往这个sock实例里面根据所采用的应用程报文格式写入数据. 这里我们使用的应用层协议是HTTP.

WEBROOT是供外界访问文件的所在目录.
PORT端口号

原理很简单,  直接看代码就好了.

```java
package com.server.simpleHttpServer;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class SimpleHttpServer {
    private final int PORT;
    private final String WEBROOT;

    public static final String RIGHT_HEADER = "HTTP/1.1 200 OK\r\nContent-Type: text/html;charset=UTF-8\r\nConnection: close\r\n\r\n";
    public static final String ERROR_HEADER = "HTTP/1.1 404 Not Found\r\nContent-Type: text/html;charset=UTF-8\r\nConnection: Close\r\n\r\n";
    public static final String NOTFOUND_CONTENT = "<html><head><title>Blast</title></head> <body> Sorry, 404 Not Found </body> </html>";

    private final ServerSocket server;

    public SimpleHttpServer(int PORT, String WEBROOT) throws IOException {
        this.PORT = PORT;
        this.WEBROOT = WEBROOT;
        server = new ServerSocket(PORT);
    }

    public void start() throws IOException {
        while (true) {
            Socket socket = server.accept();
            System.out.println("得到一个来自 " + socket.getRemoteSocketAddress() + " 的连接");
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            String bf;
            StringBuilder sb = new StringBuilder();

            while (br.ready() && (bf = br.readLine()) != null) {
                sb.append(bf);
            }
            System.out.println("req " + sb.toString());
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            try {
                String filePath = getUri(sb.toString());
                if (filePath == null) {
                    bw.write(ERROR_HEADER);
                    bw.write(NOTFOUND_CONTENT);
                } else {
                    String content = getFile(filePath);
                    bw.write(RIGHT_HEADER);
                    bw.write(content);
                }
            } catch (IOException | StringIndexOutOfBoundsException e) {
                e.printStackTrace();
                bw.write(ERROR_HEADER);
                bw.write(NOTFOUND_CONTENT);
            }
            bw.flush();
            br.close();
            bw.close();
            socket.close();
        }
    }


    private String getUri(String firstLine) {
        if (firstLine == null || firstLine.length() == 0) {
            return null;
        }
        int i = firstLine.indexOf(" ");
        int j = firstLine.indexOf(" ", i + 1);
        return firstLine.substring(i + 1, j);
    }

    private String getFile(String filePath) throws IOException {
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(new FileInputStream(WEBROOT + filePath)))) {
            StringBuilder sb = new StringBuilder();
            String buf = null;
            while ((buf = br.readLine()) != null) {
                sb.append(buf);
            }
            br.close();
            return sb.toString();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
            throw new FileNotFoundException();
        }
    }
}

```

其实在写这个的时候还是碰到了一个大坑的.

起初, 我用bufferedReader读的时候. 是这样的.

```java
            while ((bf = br.readLine()) != null) {
                sb.append(bf);
            }
```

然后, 在处理一个进来的连接的时候, 都会阻塞在这里一会. 当时debug了好长时间, 竟然没发现, 哭了....

然后在stackovreflow上找到了解决方案.  st地址:https://stackoverflow.com/questions/15521352/bufferedreader-readline-blocks

然后加上了`ready()`方法, `ready()`方法确保在输入流准备好了之后, 才会返回`true`, 所以写在`readline()`方法前, 快速失败, 避免阻塞线程.

```java
public class Main {
    public static void main(String[] args) throws IOException {
        sStart();
    }
    private static void sStart() throws IOException {
        SimpleHttpServer server = new SimpleHttpServer(8080, "C:\\Users\\youngxinler\\Desktop");
        server.start();
    }
}
```

运行一下.

访问我放在桌面的这个`index.html`
```html
<html>
<head>
    <title>HTTP-Server</title>
</head>
<body>
    welcome to blast!
</body>
</html>
```

![结果](https://i.loli.net/2019/11/17/j3OZ6hczEgmR15C.png)

当然,  这个同时处理一个连接,  如果一个连接处理时间过程很长, 后续连接就会被阻塞, 很影响响应度.

下面给出多线程版本.

### 多线程版本

一个主线程来接受sock连接, 然后再生成新的线程进行处理.

负责处理的线程

```java
package com.server.multiBlockHttpServer;

import java.io.*;
import java.net.Socket;

public class ConnectionHandler extends Thread {
    static String webRoot;
    private Socket s;

    public static final String RIGHT_HEADER = "HTTP/1.1 200 OK\r\nContent-Type: text/html;charset=UTF-8\r\nConnection: close\r\n\r\n";
    public static final String ERROR_HEADER = "HTTP/1.1 404 Not Found\r\nContent-Type: text/html;charset=UTF-8\r\nConnection: Close\r\n\r\n";
    public static final String NOTFOUND_CONTENT = "<html><head><title>Blast</title></head> <body> Sorry, 404 Not Found </body> </html>";

    public ConnectionHandler(Socket s) {
        this.s = s;
    }

    @Override
    public void run() {
        while (!s.isClosed()) {
            try {
                BufferedReader br = getBufReader(s);
                BufferedWriter bw = getBufWriter(s);

                String buf;
                StringBuilder sb = new StringBuilder();
                while (br.ready() && (buf = br.readLine()) != null) {
                    sb.append(buf);
                }

                System.out.println("req " + sb.toString());

                try {
                    String filePath = getUri(sb.toString());
                    if (filePath == null) {
                        bw.write(ERROR_HEADER);
                        bw.write(NOTFOUND_CONTENT);
                    } else {
                        String content = getFile(filePath);
                        bw.write(RIGHT_HEADER);
                        bw.write(content);
                    }
                } catch (IOException | StringIndexOutOfBoundsException e) {
                    e.printStackTrace();
                    bw.write(ERROR_HEADER);
                    bw.write(NOTFOUND_CONTENT);
                }

                bw.flush();
                br.close();
                bw.close();
                s.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    private String getUri(String firstLine) {
        if (firstLine == null || firstLine.length() == 0) {
            return null;
        }
        int i = firstLine.indexOf(" ");
        int j = firstLine.indexOf(" ", i + 1);
        return firstLine.substring(i + 1, j);
    }

    private String getFile(String filePath) throws IOException {
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(new FileInputStream(webRoot + filePath)))) {
            StringBuilder sb = new StringBuilder();
            String buf = null;
            while ((buf = br.readLine()) != null) {
                sb.append(buf);
            }
            br.close();
            return sb.toString();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
            throw new FileNotFoundException();
        }
    }

    private BufferedReader getBufReader(Socket s) throws IOException {
        return new BufferedReader(new InputStreamReader(s.getInputStream()));
    }

    private BufferedWriter getBufWriter(Socket s) throws IOException {
        return new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
    }
}
```

server类

```java
package com.server.multiBlockHttpServer;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class MultiBlockHttpServer {
    private ExecutorService executorService;
    private ServerSocket serverSocket;
    private final String WEBROOT;

    public MultiBlockHttpServer(int port, int nThread, String webRoot) throws IOException {
        executorService = Executors.newFixedThreadPool(nThread);
        serverSocket = new ServerSocket(port);
        this.WEBROOT = webRoot;
    }

    public void start() throws IOException {
        System.out.println("Start blast!");
        ConnectionHandler.webRoot = WEBROOT;
        while (true) {
            Socket s = serverSocket.accept();
            System.out.println("来自 " + s.getRemoteSocketAddress() + " 的连接");
            executorService.submit(new ConnectionHandler(s));
        }
    }
}
```

这样,  主线程负责接收连接, 然后就不用管它了. 剩下的交给新的线程来处理.

但是这里也是有极大性能问题的, 因为我们可以即使开出来很多的线程, 但是线程切换上下文是非常耗时, 而且大量的连接, 可能会生成很多线程, 一个线程大概占用1M内存, 所以, 在高并发下, 可能会直接OutOfMemoryError.

但是像nginx确是单线程性能却高的鸭皮,  这里就涉及到Nio了. 

等我有时间再撸一个Nio版本的出来.








