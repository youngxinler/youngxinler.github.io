---
title: Two-Phase Termination模式
tags: [设计模式, 多线程, Java]
---

#### 适用的情况
当想要终止正在运行的线程, 如果突然被紧急终止了, 那么这时候的实例的状态可能就会出现错误.
#### 实现的方式
可能会被中断的线程, 轮询线程的状态或者捕获InterruptException进行处理, 利用finally{}确保线程关闭的时候维护相关状态的安全.
#### 相关的模式
- 当想在执行终止处理前禁止其他处理, 可以使用[Balking模式](https://www.jianshu.com/p/ca657318446b).
#### 代码示例:
>这是一个负责慢慢累加的CountUpThread"线程"类, 他会在接收到终止通知的时候, 打印最后counter的大小.

[图片上传失败...(image-248996-1562330875965)]


```java
package com.graphic.twoPhaseTermination;

/**
 * @author youngxinler  19-6-4 下午5:36
 * @version 0.1
 * <p>
 * 这里有一点不懂, 如果去掉shutdownRequested变量, 那么由无论是内部还是外部去调用isInterrupted()方法, 判断线程是否中断的话, isInterrupted()方法的返回结果一直是false.
 * <p>
 * 加上shutdownRequested方法的话, 使用isShutdownRequested()来判断"逻辑上的线程中断"是好使的, 但是为什么isInterrupted()就不行呢? 这难道是个"鬼才方法"? 调用isInterrupted()会自动结束线程的中断状态?
 **/


public class CountUpThread extends Thread {
    private long counter = 0;

    private volatile boolean shutdownRequested = false;

    public void shutdown() {
        shutdownRequested = true;
        interrupt();
    }

    public boolean isShutdownRequested() {
        return shutdownRequested;
    }

    @Override
    public void run() {
        try {
            while (true) {
                doWork();
            }
        } catch (InterruptedException e) {
            System.out.println("i get the interruptedException");
            System.out.println("self check counterThread isInterrupt " + isInterrupted());
            interrupt();
        } finally {
            doShutdown();
        }
    }


    private void doWork() throws InterruptedException {
        counter++;
        System.out.println("doWork: counter = " + counter);
        Thread.sleep(500);
    }

    private void doShutdown() {
        System.out.println("doShutdown: counter = " + counter);
    }
}
```


```java
package com.graphic.twoPhaseTermination;

/**
 * @author youngxinler  19-6-4 下午5:47
 * @version 0.1
 **/

public class Main {
    public static void main(String[] args) {
        System.out.println("main : start");
        try {
            CountUpThread countUpThread = new CountUpThread();
            countUpThread.start();

            Thread.sleep(10000);
            System.out.println("main : shutdownRequest");
            countUpThread.shutdown();


            System.out.println("main : join");
            countUpThread.join();
            System.out.println("main check counterThread isShutdownRequested " + countUpThread.isShutdownRequested());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("main : end");
    }
}
```

