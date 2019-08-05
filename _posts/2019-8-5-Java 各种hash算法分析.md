---
title: 2019-8-5-Java 各种hash算法分析
tags: Java
---
>Hash, 把任意长度的输入, 通过散列算法, 转化为一个定长的值.
>散列算法的一个特性:*根据同一散列算法算出来的值, 如果输入值是不同的, 那么输出值也不同, 但是根据同一函数算出来的散列值相同, 那么输入值不一定相同.(哈希冲突)*


根据散列算法的特性, javadoc中说明了java `hashcode`的准则:
概括来说就是如果equals()方法返回true, 那么生成的hashcode一定相同, 如果equals()方法返回false, 那么生成的hashcode则可能相同也可能不同.(*这也是为什么javadoc上要求, equals()方法和hashcode()方法尽量一起重写的原因*)

看起来有点绕口, 再简略一下, **hashcode相等是两个实例相等的前提条件.**

在Object(所有类型的根类)里就有了一个`JNI`的`public native int hashCode()`的方法, 这样的hash算法在多数情况下给了实例之间相互进行比较的一个前提变量,  优化了进行比较时候所产生的性能开销. 该JNI方法通过将实例的内存地址转化为int.但是不等于直接的内存地址.

以下源码, 如没有明确指明, 就是openjdk8中的源码.

### String的hashcode()
String是我们日常几乎离不开的一个类, 而且是Immutable的, 所以先来看看String的重写

```java

    /** Cache the hash code for the string */
    private int hash; // Default to 0
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }

```

首先String是Immutable(不可变)的, 所以初始化之后, hashcode也就随之确定了, 类中有一个字段`hash`来保存着不会变化的哈希值, 反正不会变, 肯定计算一次就够了, 保存下来, 以后调用的时候直接返回就行了. 所以`if(h == 0 &&h == 0 && value.length > 0)`避免了哈希值的二次计算和空字符串的计算.


可以看出hash=s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]  , 但是为什么要这么做呢? javadoc中并没有给出, 但是这就是研究源码的目的所在, 所以就去查.


在stackoverflow上最高票的解答是[Why does Java's hashCode() in String use 31 as a multiplier?](https://stackoverflow.com/questions/299304/why-does-javas-hashcode-in-string-use-31-as-a-multiplier) 

这个人也是引用了<\<Effective Java>>中的观点. 下面解释下:

>选择值31是因为它是奇数和素数。如果它是偶数并且乘法溢出，则信息将丢失，因为乘以2相当于移位。使用素数的优势不太明显，但这是传统的思路。 31的一个很好的属性是乘法可以用移位和减法代替以获得更好的性能：31 * i ==（i << 5） - i。JVM会对这个乘法自动进行优化.


>大多数hash算法都会选择与素数进行运算, 这样能避免hash冲突, 31一个不大不小的素数, 是个很好的选择. 最重要的JVM会对此进行优化! 


### HashMap的hash算法
HashMap是基于`数组+链表\红黑树`的一种常用的Map结构. HashMap的数组初始长度是16, 之后每次扩展为原来的两倍. 这是要说HashMap的hash算法的前提知识, 如果你对此不太了解, 建议先去了解一下.

HashMap为了避免hash冲突, 没有直接采用Key的hash值的默认返回, 而对其进行了二次散列运算, 看下源码:

```java
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
	// >>为有符号右移, >>>为无符号右移. 有符号会保持原数的正负性, 而无符号则不会, 默认补0
```
HashMap支持唯一的key为null, 并且从源码中可以看出来, 默认的键值为null的数组下标为0.
如果key非null, 则拿key的hashcode与其本身key的hashcode的高16位的值做幂运算.

为什么好好的key的默认返回值不用, 非要二次运算呢?(扰乱运算)
先别着急, 这要根据hashmap寻找数组index的结合起来一起说较容易理解.

如果是"屌丝"写法, 那么肯定是hash%length, 这样就能得到table数组下标, 但是写jdk能是屌丝么??? (滑稽)
先看下jdk7中的寻找数组下标的实现, 通过与数组长度求与运算得到具体的数组下标, 这里的原理是针对x & 2^n-1 (hashmap的数组大小就是2^n) 的与运算, 是和x % n的运算结果是一样的, 但是&运算肯定比%效率要高的多, 举个例子:  
6 % 8 = 6                ==              6 & 7 = 6  
6 : 000110<br>
7 : 000111<br>
6 : 000110<br>
针对于其他的2^n-1, 结果也都是一样的.
所以, 大佬就是大佬, 比不了啊,

```java
//jdk7
static int indexFor(int h, int length) {
    return h & (length-1);
}

//jdk8
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```

jdk8中的没有了indexFor()函数, 而是直接在寻找node节点中的函数实现, 具体的就是:
```java
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
```
就是上面中的`[(n-1)&hash] `, hash是key的最终hash值.
这里假设n-1 = 15, hash = 6, 我们来看下 6 % 16 = 6  == 6 & (16 -1)
00000000000000000000000000001111<br>
00000000000000000000000000000110<br>
00000000000000000000000000000110<br>
这里要注意, 0与任何数求与都是0, 也就是无论hash是多少, 数组的长度的二进制位数代表了最终参与 `与运算`的位数.
好了分析了数组的下标寻址过程, 这样我们就能说下为什么key的hash值要二次计算.
首先看下  
10101010101101010011100101000101<br>
00000000000000000000000000000101
比较一下这两个hash, 如果数组的长度是16, 那么只有hash的后4位参与`与运算`, 但是着两个是相差很大数啊,  **所以为了避免数组寻址与运算造成的频繁"低位"哈希冲突, 所以key的hash值进行了二次运算**,  这里的二次哈希计算是一种扰乱函数, ` (h = key.hashCode()) ^ (h >>> 16)`**作用是让高位也参与到数组的确定下标运算中, 从而避免了确定地址的运算只依赖于低位**. (这里的低位和高位是key的默认hash值的二进制的表达形式)

真🐮,  这种对代码精益求精的态度. 这也保证了我们平时用的数据结构的算法是最优的, 感谢这群人. 