---
title: Thread-Local Storage模式
tags: [设计模式, 多线程, Java]
---

#### 别名
- Per-Thread Attribute
- Thread-Specific Data
- Thread-Specific Field
- Thread-Local Storage
#### 适用的情况
使每个线程拥有独立的上下文实例. 从而避免了多线程之间的实例竞争.
#### 实现的方式
**java.lang.ThreadLocal**来保管相关的所有线程的单独实例.
#### 代码示例:
>Log利用ThreadLocal保存着所有线程单独的TSLog对象, 然后每个线程通过TSLog打印各自的日志到各自本地的日志文件.

```java
package com.graphic.threadSpecificStorage;

import java.io.File;
import java.io.IOException;
import java.io.PrintWriter;

public class TSLog {
    private PrintWriter printWriter = null;

    public TSLog(String fileName) {
        try {
            printWriter = new PrintWriter(new File(fileName));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void println(String s) {
        printWriter.write(s);
    }

    public void close() {
        printWriter.write("===  END OF Log  ===");
        printWriter.close();
    }
}
```

```java
package com.graphic.threadSpecificStorage;

public class Log {
    private static final ThreadLocal<TSLog> tsLogCollection = new ThreadLocal<TSLog>();

    public static void println(String s) {
        getTSLog().println(s);
    }

    public static void close() {
        getTSLog().close();
    }

    private static TSLog getTSLog() {
        TSLog tsLog = tsLogCollection.get();
        if (tsLog == null) {
            tsLog = new TSLog(Thread.currentThread().getName() + "-log.txt");
            tsLogCollection.set(tsLog);
        }
        return tsLog;
    }
}
```
```java
package com.graphic.threadSpecificStorage;

public class ClientThread extends Thread {
    public ClientThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println(getName() + "    Begin");
        for (int i = 0; i < 10; i++) {
            Log.println("i = " + i);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Log.close();
        System.out.println(getName() + "    END");
    }
}
```
```java
package com.graphic.threadSpecificStorage;

public class Main {
    public static void main(String[] args) {
        new ClientThread("alice").start();
        new ClientThread("bob").start();
        new ClientThread("chris").start();
    }
}
```

