线程正确性<br>
======
---------------------------
什么是线程安全性？“安全”的要以是什么？安全的定义中，最核心的概念就是正确性。那么什么是正确性？<br>
  >`单线程`执行过后的结果近似为“所见即所知”（we know it when we see it)"，我们将之称为正确性。

  >当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在`主调代码`中不需要任何额外的同步或协同，这个类都能表现出正确的行为，那么就称这个类是线程安全的。

锁<br>
======
-------------------------------------------

最常见的线程同步方法就是加锁，锁的特点有：<br>
- 互斥（mutual exclusion）
>互斥即一次只允许一个线程持有某个特定的锁，因此可使用该特性实现对共享数据的协调访问协议，这样，一次就只有一个线程能够使用该共享数据。

- 内存可见性（visibility）
>可见性要更加复杂一些，它必须确保释放锁之前对共享数据做出的更改对于随后获得该锁的另一个线程是可见的。

synchronized关键字<br>
======
-------------------------------------------------
- 当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。<br>
- 然而，当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块。<br>
- 当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞。<br>
- **也就是说，当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。**<br>
- 以上规则对其它对象锁同样适用.<br>

警告<br>
======
--------------------------
即使类的所有方法都是`synchronized`修饰的，也并不能保证调用这些方法组合时候的原子性，复合操作还是需要进行另外的同步。<br>

那么对于`static synchronized`与`synchronized`又有什么区别呢？<br>
>实际上，在类中某方法或某代码块中有 synchronized，那么在生成一个该类实例后，改类也就有一个监视快，放置线程并发访问改实例synchronized保护快，而static synchronized则是所有该类的实例公用一个监视快了，也也就是两个的区别了,也就是synchronized相当于 this.synchronized，而static synchronized相当于Something.synchronized。

[synchronized与static synchronized 的区别](http://www.cnblogs.com/shipengzhi/articles/2223100.html)。

volatile<br>
======
-----------------------------
`volatile`用到的也非常的频繁，但是该关键字在使用的时候需要特别的小心.Volatile 变量具有 synchronized 的可见性特性，但是`**不具备原子特性**`。
其使用的前置条件有：<br>
- 对变量的写操作不依赖于当前值。
- 该变量没有包含在具有其他变量的不变式中。

[正确使用 Volatile 变量](https://www.ibm.com/developerworks/cn/java/j-jtp06197.html)<br>

关于wait,notify方法<br>
======
---------------------------
wait、notify和notifyAll方法是Object类的final native方法。所以这些方法不能被子类重写，Object类是所有类的超类，因此在程序中有以下三种形式调用wait等方法。<br>
**void notifyAll()**<br>
解除所有那些在该对象上调用wait方法的线程的阻塞状态。该方法只能在同步方法或同步块内部调用。如果当前线程不是锁的持有者，该方法抛出一个IllegalMonitorStateException异常。<br>

**void notify()**<br>
随机选择一个在该对象上调用wait方法的线程，解除其阻塞状态。该方法只能在同步方法或同步块内部调用。如果当前线程不是锁的持有者，该方法抛出一个IllegalMonitorStateException异常。<br>

**void wait()**<br>
导致线程进入等待状态，直到它被其他线程通过notify()或者notifyAll唤醒。该方法只能在同步方法中调用。如果当前线程不是锁的持有者，该方法抛出一个IllegalMonitorStateException异常。<br>

**void wait(long millis)和void wait(long millis,int nanos)**<br>
导致线程进入等待状态直到它被通知或者经过指定的时间。这些方法只能在同步方法中调用。如果当前线程不是锁的持有者，该方法抛出一个IllegalMonitorStateException异常。<br>

Object.wait()和Object.notify()和Object.notifyall()必须写在synchronized方法内部或者synchronized块内部，这是因为：这几个方法要求当前正在运行object.wait()方法的线程拥有object的对象锁。即使你确实知道当前上下文线程确实拥有了对象锁，也不能将object.wait()这样的语句写在当前上下文中。
