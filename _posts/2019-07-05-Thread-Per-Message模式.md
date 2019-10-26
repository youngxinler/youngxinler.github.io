---
title: Thread-Per-Message模式
tags: [设计模式, 多线程, Java]
---
#### 适用的情况
解决一个请求会花费比较长的时间, 这时候程序的主进程的控制权一直会被当前请求所占用, 其他的请求也无法进行处理.
#### 实现的方式
在接收请求的主线程方法中, 另外开启一个线程, 让新开启的线程对该请求进行处理, 这时候, 接收请求的主线程不会被阻塞到.
#### 相关的模式
- 想要节省开启线程所花费的时间, 可以使用[Worker-Thread模式](https://www.jianshu.com/p/eb5c4467a1ec).
- 当需要返回结果的时候, 可以使用[Future模式](https://www.jianshu.com/p/7aa74bc8fb40).
#### 代码示例:
>Host接收请求, 然后开启新的线程进行任务的处理, (字符串输出, 为了模拟正常情况下, 进行Thread.sleep()阻塞).

```java
package com.graphic.threadPerMessage;

/**
 * @author youngxinler  19-6-2 上午10:09
 * @version 0.1
 **/

public class Helper {
    public void handle(int count, char c) {
        System.out.println("    handle begin " + c);
        for (int i = 0; i < count; i++) {
            slowly();
            System.out.print(c);
        }
        System.out.println();
        System.out.println("    handle end " + c);
    }

    private void slowly() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
```java
package com.graphic.threadPerMessage;

/**
 * @author youngxinler  19-6-2 上午10:17
 * @version 0.1
 **/


public class Host {
    private final Helper helper = new Helper();

    public Host() {
    }

    //如果内部类使用了方法参数, 那么必须对参数加上final, 否则会报错
    public void request(final int count, final char c){
        System.out.println("request begin " + count + " " + c);

        //  每个request 委托新的线程去执行
        // 适合如果处理的时间特别长, 而且对操作顺序和返回值没有要求
        new Thread(){
            @Override
            public void run(){
                helper.handle(count, c);
            }
        }.start();
        System.out.println("request end " + count + " " + c);
    }
}
```

```java
package com.graphic.threadPerMessage;

import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

/**
 * @author youngxinler  19-6-2 上午10:20
 * @version 0.1
 **/

public class Main {
    public static void main(String[] args) {
        Host host = new Host();
                try {
            host.request(50, 'A');
            host.request(50, 'B');
            host.request(50, 'C');
        } finally {
            System.out.println("main end");
        }
	}
}
```

