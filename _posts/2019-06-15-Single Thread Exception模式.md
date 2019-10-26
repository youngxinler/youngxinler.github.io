---
title: Single Thread Exception模式
tags: [设计模式, 多线程, Java]
---

#### 适用的情况
当多个线程同时对可变实例进行操作的情况下,  实例就会变成非线程安全.

#### 实现的方式
**Synchronized关键字**  
synchronized使得指定的方法或者代码块只能串行执行, 也就是同一时间段, 只能有一个线程能对synchronized锁定的地方进行访问.
 - synchronized 声明在方法上
	默认锁定的对象是this
 - synchronized 声明在代码块上
 	要指定要锁定的对象, 或者指定锁定的标志
>多个线程同时访问synchonized加锁的方法或者代码段,  就会发生阻塞, 除了获得锁的线程, 其他线程会进入锁定对象的同步队列, 获得锁的线程完成相关任务后, **随机唤醒**一个等待队列里面的线程, 获得锁. 以此类推.

#### 相关的模式
- 当共享实例的数据不会发生变化的时候, 可以使用Immutable模式.
- 读的次数要远远大于写的情况下, 可以进行读写分离,  使用Read-Write Lock模式

#### 代码示例:
```java
package com.graphic.singleThreadException;


public class Gate {
    private int counter = 0;
    private String name = "NoBody";
    private String address = "NoWhere";

    public synchronized void pass(String name, String address){
        this.counter++;
        this.name = name;
        this.address = address;
        check();
    }

    /*
     * why? toString(）方法是public， 持有该实例的“代码块”都可以进行调用， 虽然在本次逻辑测试当中去掉也没事， 但是为了完全通用Gate类，
     * 必须进行synchronized， 因为一旦在pass方法执行过程中，调用了toString()方法， 那么就造成了“脏读”， 可能会出现 ： Alice, NoWhere
     */
    @Override
    public synchronized String toString(){
        return "No." + counter + ": " + name + ", " + address;
    }

    private void check(){
        if (name.charAt(0) != address.charAt(0)){
            System.out.println("***** BROKEN *****" + toString());
        }
    }
}

```

```java
package com.graphic.singleThreadException;

public class UserThread extends Thread {
    private final Gate gate;
    private final String myName;
    private final String myAddress;

    public UserThread(Gate gate, String myName, String myAddress) {
        this.gate = gate;
        this.myName = myName;
        this.myAddress = myAddress;
    }

    @Override
    public void run(){
        System.out.println(myName + " BEGIN");
        while (true){
            gate.pass(myName, myAddress);
        }
    }
}

```

```java
package com.graphic.singleThreadException;

public class Main {
    public static void main(String[] args) {
        System.out.println("Testing Gate, hit CTRL+C to exit");
        Gate gate = new Gate();
        new UserThread(gate, "Alice", "Alaska").start();
        new UserThread(gate, "Bobby", "Brazil").start();
        new UserThread(gate, "Chris", "Canada").start();
    }
}
```
