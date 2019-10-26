---
title: Worker Thread模式
tags: [设计模式, 多线程, Java]
---

#### 别名
- Thread Pool
- Background Thread

#### 适用的情况
为了提高响应性， 而经常开启新线程让他负责活动的处理，但是每次开启关闭线程都需要花费时间.

#### 实现的方式
在活动的开始就启动多个线程存放起来, 然后将请求发送给这些线程进行处理.这样就不用每次接受请求的时候再进行创建和关闭线程的工作了.

#### 相关的模式
- 在将工人线程的处理结果返回给调用的方法时候， 可以使用[Future模式](https://www.jianshu.com/p/7aa74bc8fb40).
- 将请求发送给线程池的缓冲区可以使用[Producer-Consumer模式](https://www.jianshu.com/p/14d88ea507b2).

#### 代码示例:
>示例说明:Channel是请求发送的"管道", Request代表了每个请求, 然后WorkerThread中执行请求的方法.ClientThread模拟了客户端请求的发送.

```java
package com.graphic.workerThread;

public class Channel {
    private static final int MAX_REQUEST = 100;
    private final Request[] requests;
    private int tail;
    private int head;
    private int count;

    private final WorkerThread[] threadPool;

    public Channel(int threadNum) {
        this.requests = new Request[MAX_REQUEST];
        this.threadPool = new WorkerThread[threadNum];
        this.head = 0;
        this.tail = 0;
        this.count = 0;

        for (int i = 0; i < threadNum; i++) {
            threadPool[i] = new WorkerThread("Worker-" + i, this);
        }
    }

    public void startWorkers() {
        for (WorkerThread w :
                threadPool) {
            w.start();
        }
    }

    public synchronized void putRequest(Request request) {
        while (count >= requests.length) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        requests[tail] = request;
        tail = (tail + 1) % requests.length;
        count++;
        notifyAll();
    }

    public synchronized Request takeRequest() {
        while (count <= 0) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Request request = requests[head];
        head = (head + 1) % requests.length;
        count--;
        notifyAll();
        return request;
    }
}

```
```java
package com.graphic.workerThread;

import java.util.Random;

public class Request {
    private final String name;
    private final int number;
    private static final Random random = new Random();

    public Request(String name, int number) {
        this.name = name;
        this.number = number;
    }

    public void execute() {
        System.out.println(Thread.currentThread().getName() + " execute " + this);
        try {
            Thread.sleep(random.nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Override
    public String toString() {
        return "[ Request from " + name + " No. " + number + " ]";
    }
}

```
```java
package com.graphic.workerThread;

public class WorkerThread extends Thread {
    private final Channel channel;

    public WorkerThread(String name, Channel channel) {
        super(name);
        this.channel = channel;
    }

    @Override
    public void run() {
        while (true) {
            Request request = channel.takeRequest();
            request.execute();
        }
    }
}
```

```java

package com.graphic.workerThread;

import java.util.Random;

public class ClientThread extends Thread {
    private final Channel channel;
    private final Random random = new Random();

    public ClientThread(String name, Channel channel) {
        super(name);
        this.channel = channel;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; true; i++) {
                Request request = new Request(getName(), i);
                channel.putRequest(request);
                Thread.sleep(random.nextInt(1000));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```java
package com.graphic.workerThread;

public class Main {
    public static void main(String[] args) {
        Channel channel = new Channel(5);
        channel.startWorkers();
        new ClientThread("alice", channel).start();
        new ClientThread("bob", channel).start();
        new ClientThread("blank", channel).start();
    }
}
```


