---
title: Read-Write Lock模式
tags: [设计模式, 多线程, Java]
---
#### 适用的情况
多个线程共享了实例, 实例 是可变的, 对实例进行读的次数大于写的次数.多个线程可以同时读, 但一段时间内只能有一个线程可以进行写.
#### 实现的方式
引入一个ReadWriteLock角色管理前来读和写的线程, 进行互斥处理. 
#### 相关的模式
- ReadWriteLock进行互斥的部分使用的是[Guarded Suspension模式](https://www.jianshu.com/p/18285cfb9a52).
- 当实例是不可变的情况下, 可以使用[Immutable模式](https://www.jianshu.com/p/47f7142cb453).
#### 代码示例:
>示例介绍:ReaderThread负责读取Data实例中的字符串, WriterThread每个一段时间向Data写入字符串, ReadWriteLock注入Data中, 对Data的读取和写入方法加入了锁.

>Notice: 使用finally确保方法即使出现异常, 最后也能关闭掉锁, 以防止出现死锁.

```java
package com.graphic.readWriteLock;

/**
 * @author youngxinler  19-6-1 下午7:49
 * @version 0.1
 **/

public final class ReadWriteLock {
    private int readingReaders = 0;
    private int waitingWriters = 0;
    private int writingWriters = 0;
    private boolean preferWriter = true;

    /*
     * 如果按照这样来写.
     *while (writingWriters > 0) {
     *     wait();
     *   }
     *读进程多于写入进程, 并且读进程是没有互斥处理的, 也就是在一开始, 读进程就在占用住table,
     *即使有一个进程在读, 写进程就进不来
     *
     */
    public synchronized void readLock() throws InterruptedException {
        while (writingWriters > 0 || (waitingWriters > 0 && preferWriter)) {
            wait();
        }
        notifyAll();
    }

    public synchronized void readUnlock() {
        readingReaders--;
        preferWriter = true;
        notifyAll();
    }

    //finally 确保
    public synchronized void writeLock() throws InterruptedException {
        waitingWriters++;
        try {
            while (readingReaders > 0 || writingWriters > 0) {
                wait();
            }
        } finally {
            waitingWriters--;
        }
        writingWriters++;
    }

    public synchronized void writeUnlock() {
        writingWriters--;
        preferWriter = false;
        notifyAll();
    }
}
```

```java
package com.graphic.readWriteLock;

import java.util.Random;

/**
 * @author youngxinler  19-6-1 下午8:07
 * @version 0.1
 **/

public class WriterThread extends Thread {
    private static final Random random = new Random();
    private final Data data;
    private final String filler;
    private int index = 0;

    public WriterThread(Data data, String filler) {
        this.data = data;
        this.filler = filler;
    }

    public void run() {
        try {
            while (true) {
                char c = nextChar();
                data.write(c);
                Thread.sleep(random.nextInt(3000));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private char nextChar() {
        char c = filler.charAt(index++);
        if (index >= filler.length()) {
            index = 0;
        }
        return c;
    }
}
```
```java
package com.graphic.readWriteLock;

/**
 * @author youngxinler  19-6-1 下午8:12
 * @version 0.1
 **/

public class ReaderThread extends Thread {
    private final Data data;

    public ReaderThread(Data data) {
        this.data = data;
    }

    @Override
    public void run() {
        try {
            while (true) {
                char[] readbuf = data.read();
                System.out.println(Thread.currentThread().getName() + " reads " + String.valueOf(readbuf));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```java
package com.graphic.readWriteLock;



/**
 * @author youngxinler  19-6-1 下午7:49
 * @version 0.1
 **/

public class Data {
    private final char[] buffers;
    private final ReadWriteLock lock = new ReadWriteLock();

    public Data(int size) {
        this.buffers = new char[size];
        for (int i = 0; i < buffers.length; i++) {
            buffers[i] = '*';
        }
    }

    public char[] read() throws InterruptedException {
        lock.readLock();
        try {
            return doRead();
        } finally {
            lock.readUnlock();
        }
    }

    public void write(char c) throws InterruptedException {
        lock.writeLock();
        try {
            doWrite(c);
        } finally {
            lock.writeUnlock();
        }
    }

    private void doWrite(char c) {
        for (int i = 0; i < buffers.length; i++) {
            buffers[i] = c;
            slowly();
        }
    }


    private char[] doRead() {
        char[] newbuf = new char[buffers.length];
        for (int i = 0; i < buffers.length; i++) {
            newbuf[i] = buffers[i];
        }
        slowly();
        return newbuf;
    }

    private void slowly() {
        try {
            Thread.sleep(50);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
package com.graphic.readWriteLock;

/**
 * @author youngxinler  19-6-1 下午8:15
 * @version 0.1
 **/

public class Main {
    public static void main(String[] args) {
        Data data = new Data(10);
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new WriterThread(data, "ABCDEFG").start();
        new WriterThread(data, "abcdefg").start();
    }
}
```
