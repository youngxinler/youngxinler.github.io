---
title: Immutable模式
tags: [设计模式, 多线程, Java]
---

#### 适用的情况
多个线程之间共享对象, 但是该对象的状态不会发生变化
用于提高[Single Thread Exception](https://www.jianshu.com/p/0ed7102c01f3)模式的吞吐量

#### 实现的方式
使对象的状态不可发生变化, 从而使其线程安全
private / final 关键字

#### 相关的方式
- 对象的状态是可以改变的, 那么可以使用[Single Thread Exception](https://www.jianshu.com/p/0ed7102c01f3)模式
- 读的次数要远远大于写的情况下, 可以进行读写分离,  使用Read-Write Lock模式

#### 代码示例:
```java
package com.graphic.immutable;

/**
 * @author youngxinler  19-5-19 下午8:26
 * @version 0.1
 *
 * 通过final关键字， 确保了只能在字段初始化或类的构建方法中能够对final字段进行赋值操作
 * 而且java保证类的构造方法是一个原子操作， 所以在实例初始化后， 需要多线程进行操作的字段不可变的
 * 从而确保了多线程的安全性
 * String 类经过实例初始化后也是不可变的！
 **/

public class Person {
    private final String name;
    private final String address;

    public Person(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName(){
        return name;
    }

    public String getAddress() {
        return address;
    }

    @Override
    public String toString() {
        return "[ Person: name = " + name + ", address = " + address + " ]";
    }
}

```
>**Notice:**
>java字段保存的都是对象的引用, (除了java的内置类型), 如果该引用的地址被其他对象获取, 就可以对该地址保存的对象进行修改了, 那么也就是说, Immutable模式保证的只能是字段引用的地址不发生变化, 如果引用的地址保存的对象是可变的, 那么一定要小心对待, 否则就会发生我们意想不到的破坏.
>我们这里的例子是没有问题的, 因为String类就是Immutable的.


```java
package com.graphic.immutable;

/**
 * @author youngxinler  19-5-19 下午8:29
 * @version 0.1
 **/

public class PrintPersonThread extends Thread{
    private Person person;

    public PrintPersonThread(Person person){
        this.person = person;
    }

    /*
     * "字符串" + 实例  会自动调用实例的toString()方法
     */
    @Override
    public void run() {
        while (true){
            System.out.println(Thread.currentThread().getName() + " prints " + person);
        }
    }
}
```

```java
package com.graphic.immutable;

/**
 * @author youngxinler  19-5-19 下午8:25
 * @version 0.1
 **/

public class Main {
    public static void main(String[] args) {
        Person alice = new Person("Alice", "Alaska");
        new PrintPersonThread(alice).start();
        new PrintPersonThread(alice).start();
        new PrintPersonThread(alice).start();
    }
}

```