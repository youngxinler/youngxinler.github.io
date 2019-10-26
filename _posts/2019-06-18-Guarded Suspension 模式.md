---
title: Guarded Suspension 模式
tags: [设计模式, 多线程, Java]
---

#### 适用的情况
相比于Single Thread Exception模式, 本模式加入了加入了守护条件来确保共享实例在被线程访问前是正确的状态
#### 实现的方式
如果实例是不正确的, 那么就让前来访问的线程执行wait(), 在线程恢复到正确状态的时候, 由持有锁的进程来唤醒等待的线程继续访问被守护的实例.  
**线程恢复到正确状态的时候, 一定要执行notify()或notifyAll(),否则等待的线程将永远无法被唤醒!**
#### 相关的模式
- [Single Thread Exception模式](https://www.jianshu.com/p/0ed7102c01f3)的升级版, 增加了守护条件!
- 如果希望在实例是非正确的条件下, 来访问的线程不进行等待, 而是直接返回, 可以使用[Balking模式](https://www.jianshu.com/p/ca657318446b)
#### 代码示例:

```java
package com.graphic.guardedSuspension;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Random;

/**
 * @author youngxinler  19-5-19 下午8:45
 * @version 0.1
 **/

public class RequestQueue {
    private final Queue<Request> requestQueue = new LinkedList<Request>();

    public synchronized Request getRequest(){
        while (requestQueue.peek() == null){
            try {
                wait();
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }
        return requestQueue.remove();
    }

    public synchronized void putRequest(Request request){
        requestQueue.add(request);
        notifyAll();
    }
}

```

```java
package com.graphic.guardedSuspension;

/**
 * @author youngxinler  19-5-19 下午8:37
 * @version 0.1
 **/

public class Request {
    private final String name;

    public Request(String name){
        this.name = name;
    }

    public String getName(){
        return name;
    }

    public String toString(){
        return "[ Request" + name + " ]";
    }
}

```

```java
package com.graphic.guardedSuspension;

import java.util.Random;

/**
 * @author youngxinler  19-5-19 下午9:00
 * @version 0.1
 **/

public class ServerThread extends Thread{
    private final Random random;
    private final RequestQueue requestQueue;

    public ServerThread(String name, long seed, RequestQueue requestQueue){
        super(name);
        this.random = new Random(seed);
        this.requestQueue = requestQueue;
    }

    @Override
    public void run(){
        for (int i = 0; i < 10000; i++) {
            Request request = requestQueue.getRequest();
            System.out.println(Thread.currentThread().getName() + " request " + request);
            try{
                Thread.sleep(random.nextInt(1000));
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }
    }
}

```

```java
package com.graphic.guardedSuspension;

import java.util.Random;

/**
 * @author youngxinler  19-5-19 下午8:55
 * @version 0.1
 **/

public class ClientThread extends Thread {
    private final Random random;
    private final RequestQueue requestQueue;

    public ClientThread(String s, long seed, RequestQueue requestQueue) {
        super(s);
        this.random = new Random(seed);
        this.requestQueue = requestQueue;
    }

    @Override
    public void run(){
        for (int i = 0; i < 10000; i++) {
            Request request = new Request("No," + i);
            System.out.println(Thread.currentThread().getName() + " request " + request);
            requestQueue.putRequest(request);
            try {
                Thread.sleep(random.nextInt(1000));
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }
    }
}


```


```java
package com.graphic.guardedSuspension;

/**
 * @author youngxinler  19-5-19 下午9:05
 * @version 0.1
 **/

public class Main {
    private static final String url = "";
    public static void main(String[] args) {
        RequestQueue requestQueue = new RequestQueue();
        new ClientThread("alice", 5451L, requestQueue);
        new ServerThread("bob", 1651531L, requestQueue);
    }
}
```

