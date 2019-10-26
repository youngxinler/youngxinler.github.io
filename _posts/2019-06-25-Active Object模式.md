---
title: Active Object模式
tags: [设计模式, 多线程, Java]
---
#### 别名
- Actor
- Concurrent Object
#### 适用的情况
actor是一个很抽象多线程模式, 每一个actor是线程独立并且有属于自己的状态, 多个actor互相发送消息以完成最终的任务. 你可以将actor模式理解为一个团队, 这个团队里面的个人就是一个运行在独立线程上的个体, 由他们互相交流并且单独处理自己的工作, 最后完成任务.
关于actor模式, 这里有一篇个人觉得很好的文章(英文)--[The actor model in 10 minutes](https://www.brianstorti.com/the-actor-model/)
#### 实现的方式
构造一个ActiveObject类, 来充当actor角色, 有actor角色去完成任务, 并且完成交互.
#### 相关的模式
- 实现actor角色会用到[Worker Thread模式](https://www.jianshu.com/p/eb5c4467a1ec)
- 将消息发送给actor时候,将消息放入队列, actor从队列中取消息进行处理, 用到了[Producer-Consumer模式](https://www.jianshu.com/p/14d88ea507b2).
- 将处理结果返回给最终调用方需要用到[Future模式](https://www.jianshu.com/p/7aa74bc8fb40).
#### 代码示例:
>如果不用java.util.concurrent.*, 那么将会使本模式的代码量很大, 阅读体验很差, 这里只考虑actor模式, 所以引入java.util.concurrent中已经实现的部分类.(完全自己实现的版本:GitHub连接--[Actor](https://github.com/youngxinler/JavaGrowthRoad/tree/master/MultiThread/src/com/graphic/activeObject))
>代码中ActiveObject来进行处理任务, MakerClientThread发出生产字符串的消息交给activeObject进行生产,
DisplayClientThread发送打印字符串消息交给activeObject进行打印.

```java
package com.graphic.activeObject.concurrent;

import java.util.concurrent.Future;

/**
 * @author youngxinler  19-7-3 下午4:39
 **/

public interface ActiveObject {
    Future<String> makeString(int count, char fillChar);

    void displayString(String string);

    void shutdown();
}
```

```java
package com.graphic.activeObject.concurrent;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

/**
 * @author youngxinler  19-7-3 下午4:43
 **/

public class ActiveObjectImpl implements ActiveObject {
    private final ExecutorService service = Executors.newSingleThreadExecutor();

    @Override
    public Future<String> makeString(final int count, final char fillChar) {
        class MakeStringRequest implements Callable<String> {
            public String call() {
                char[] buffer = new char[count];
                for (int i = 0; i < count; i++) {
                    buffer[i] = fillChar;
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {

                    }
                }
                return new String(buffer);
            }
        }
        return service.submit(new MakeStringRequest());
    }

    @Override
    public void displayString(final String string) {
        class DisplayStringRequest implements Runnable {
            @Override
            public void run() {
                try {
                    System.out.println("display: " + string);
                    Thread.sleep(100);
                } catch (InterruptedException e) {

                }
            }
        }
        service.submit(new DisplayStringRequest());
    }


    @Override
    public void shutdown() {
        service.shutdown();
    }
}
```
```java
package com.graphic.activeObject.concurrent;

/**
 * @author youngxinler  19-7-3 下午4:43
 **/

public class ActiveObjectFactory {
    public static ActiveObject createActiveObject() {
        return new ActiveObjectImpl();
    }
}
```
```java
package com.graphic.activeObject.concurrent;

import java.util.concurrent.Future;

/**
 * @author youngxinler  19-7-3 下午4:38
 **/

public class MakerClientThread extends Thread {
    private final ActiveObject activeObject;
    private final char fillChar;

    public MakerClientThread(String s, ActiveObject activeObject) {
        super(s);
        this.activeObject = activeObject;
        this.fillChar = s.charAt(0);
    }

    @Override
    public void run() {
        try {
            for (int i = 0; true; i++) {
                Future<String> future = activeObject.makeString(i, fillChar);
                Thread.sleep(10);
                String value = future.get();
                System.out.println(Thread.currentThread().getName() + ": value = " + value);
            }
        } catch (Exception e) {
            System.out.println(Thread.currentThread().getName() + ":" + e);
        }
    }
}
```
```java
package com.graphic.activeObject.concurrent;

import java.util.concurrent.CancellationException;
import java.util.concurrent.RejectedExecutionException;

/**
 * @author youngxinler  19-7-3 下午6:23
 **/

public class DisplayClientThread extends Thread {
    private final ActiveObject activeObject;

    public DisplayClientThread(String name, ActiveObject activeObject) {
        super(name);
        this.activeObject = activeObject;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; true; i++) {
                String s = Thread.currentThread().getName() + " " + i;
                activeObject.displayString(s);
                Thread.sleep(200);
            }
        } catch (RejectedExecutionException e) {
            System.out.println(Thread.currentThread().getName() + ":" + e);
        } catch (CancellationException e) {
            System.out.println(Thread.currentThread().getName() + ":" + e);
        } catch (InterruptedException e) {
            System.out.println(Thread.currentThread().getName() + ":" + e);
        }
    }
}
```
```java
package com.graphic.activeObject.concurrent;

/**
 * @author youngxinler  19-7-3 下午6:32
 **/

public class Main {
    public static void main(String[] args) {
        ActiveObject activeObject = ActiveObjectFactory.createActiveObject();
        try {
            new MakerClientThread("alice", activeObject).start();
            new MakerClientThread("bobby", activeObject).start();
            new DisplayClientThread("chris", activeObject).start();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

