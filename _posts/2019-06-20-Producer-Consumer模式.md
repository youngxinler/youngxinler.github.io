---
title: Producer-Consumer模式
tags: [设计模式, 多线程, Java]
---

#### 适用的情况
由多个线程之间处理生产消费的关系, 并且生产和消费不是即时处理的情况, 其中涉及到数据量的线程安全性问题.
#### 实现的方式
在Producer和Consumer之间设立一个中转站Channel, 让Channel来保存和维护数据的安全, 这样生产者和消费者之间就解耦了, 与他们有关的对象是Channel, 并且Channel是线程安全的.
#### 相关的模式
- Channel角色保证数据安全状态的时候可以使用[Guarded Suspension模式](https://www.jianshu.com/p/18285cfb9a52).
- 在[Future模式](https://www.jianshu.com/p/7aa74bc8fb40)中, 传递返回值的时候, 可以使用Producer-Consumer模式.
- [Worker-Thread模式](https://www.jianshu.com/p/eb5c4467a1ec)中, 对于Worker的请求可以使用Producer-Consumer模式对请求进行控制.
#### 代码示例:
>下面是一个例子, 由MakerThread生产字符串, Table进行保存, 然后ConsumerThread进行消费(打印出来).

```java
package com.graphic.producerAndConsumer;

import java.util.Random;

/**
 * @author youngxinler  19-6-1 上午11:40
 * @version 0.1
 **/

public class MakerThread extends Thread{
    private final Random random;
    private final Table table;
    private static int id = 0;

    public MakerThread(String name, Table table, long seed) {
        super(name);
        this.random = new Random(seed);
        this.table = table;
    }

    @Override
    public void run(){
        try {
            while (true) {
                Thread.sleep(random.nextInt(1000));
                String cake = "[Cake No." + nextId() + " by " + getName() + "]";
                table.put(cake);
            }
        }catch (InterruptedException e){
            e.printStackTrace();
        }
    }

    private int nextId(){
        return id++;
    }
}
```
```java
package com.graphic.producerAndConsumer;

import java.util.LinkedList;

/**
 * @author youngxinler  19-6-1 上午11:42
 * @version 0.1
 *
 * 这里的table其实相当于一个池子，存放着生产者生产的物品，等待消费者来消费。
 * 为什么用table来保证线程安全？
 * 1.明白要保护的变量, 这个例子中会造成线程不安全的变量是buffer[],而buffer位于table.
 * 2.与maker和consumer"断绝关系", 保证了table的线程安全, 那么对于maker和consumer就可以大胆放心的写了.
 * 3.保证了table的通用性.
 *
 **/

public class Table {
    private final String[] buffer;
    private int tail;
    private int head;
    private int count;

    public Table(int count) {
        this.buffer = new String[3];
        this.tail = 0;
        this.head = 0;
        this.count = 0;
    }

    public synchronized void put(String cake)throws InterruptedException{
        System.out.println(Thread.currentThread().getName() + " puts " + cake);
        while (count >= buffer.length){
            wait();
        }
        buffer[tail] = cake;
        tail = (tail + 1) % buffer.length;
        count++;
        notifyAll();
    }

    public synchronized String take() throws InterruptedException{
        while (count <= 0){
            wait();
        }
        String cake = buffer[head];
        head = (head + 1) % buffer.length;
        count--;
        System.out.println(Thread.currentThread().getName() + " take " + cake);
        notifyAll();
        return cake;
    }
}
```

```java
package com.graphic.producerAndConsumer;

import java.util.Random;

/**
 * @author youngxinler  19-6-1 下午12:44
 * @version 0.1
 **/

public class ConsumerThread extends Thread{
    private final Table table;
    private final Random random;

    public ConsumerThread(String s, Table table, long seed) {
        super(s);
        this.table = table;
        this.random = new Random(seed);
    }

    @Override
    public void run(){
        try{
            while (true){
                Thread.sleep(random.nextInt(1000));
                String cake = table.take();
                System.out.println(cake + " is eaten by " + getName());
            }
        }catch (InterruptedException e){
            e.printStackTrace();
        }
    }
}
```
```java
package com.graphic.producerAndConsumer;

/**
 * @author youngxinler  19-6-1 下午12:49
 * @version 0.1
 **/

public class Main {
    public static void main(String[] args) {
        Table table = new Table(3);
        new MakerThread("maker-1", table, 1000).start();
        new MakerThread("maker-2", table, 1000).start();
        new MakerThread("maker-3", table, 1000).start();
        new ConsumerThread("consumer-1", table, 1).start();
        new ConsumerThread("consumer-2", table, 2).start();
        new ConsumerThread("consumer-3", table, 3).start();
    }
}
```
