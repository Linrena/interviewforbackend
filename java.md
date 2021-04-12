### ReenTrantLock可重入锁（和synchronized的区别）

可重入性：

从名字上理解，ReenTrantLock的字面意思就是再进入的锁，其实synchronized关键字所使用的锁也是可重入的，两者关于这个的区别不大。两者都是同一个线程没进入一次，锁的计数器都自增1，所以要等到锁的计数器下降为0时才能释放锁。

锁的实现：

Synchronized是依赖于JVM实现的，而ReenTrantLock是JDK实现的，