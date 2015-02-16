哪个对象才是锁
--------

我们都知道当一个线程试图访问同步代码块时，它首先必须得到锁，退出或抛出异常时必须释放锁。这些基础也许大家都知道，但是很多人还是搞不清哪个对象才是锁？如果你能正确回答以下问题，那么才算你彻底搞明白了哪个对象才是锁？

**静态同步方法问题**

如下代码是两个静态同步方法
```java
Class A{

public static synchronized void write(boolean b){
  isTrue = b;
}

public static synchronized boolean read(){
  return isTrue;
}
}
```
那么我们来问几个问题

1. 线程1访问A.write(true)方法时，线程2能访问A.read()方法吗？
2. 线程1访问new A().write(false)方法时，线程2能访问new A().read()方法吗？
3. 线程1访问A.write(false)方法时，线程2能访问new A().read()方法吗？


**实例同步方法问题**

如下代码是两个实例同步方法
```java
public synchronized void write(boolean b){
  isTrue = b;
}

public synchronized boolean read(){
  return isTrue;
}
``
同样问两个问题：

1. A a=new A(); 线程1访问a.write(false)方法，线程2能访问a.read()方法吗？
2. A a=new A(); A b=new A();线程1访问a.write(false)方法，线程2能访问b.read()方法吗？
回答问题之前，先想一下当前方法使用的锁是哪一个？当前线程是否有拿到这把锁？拿到锁了就能访问当前方法了。

### 答案

我们先回顾基础知识，Java中的每一个对象都可以作为锁，而不同的场景锁是不一样的。

* 对于实例同步方法，锁是当前实例对象。
* 对于静态同步方法，锁是当前对象的Class对象。
* 对于同步方法块，锁是Synchonized括号里配置的对象。

1. 线程1访问A.write()方法时，线程2能访问A.read()方法吗？不能，因为静态方法的锁都是A.Class对象,线程1拿到锁之后，线程2就拿不到锁了。

2. 线程1访问new A().write()方法时，线程2能访问new A().read()方法吗？不能，原因同上。

3. 线程1访问A.write()方法时，线程2能访问new A().read()方法吗？不能，原因同上

4. A a=new A(); 线程1访问a.write()方法，线程2能访问a.read()方法吗？不能，因为这两个方法的锁都是对象a，线程1拿到了锁，线程2就不能访问了。

5. A a=new A(); A b=new A();线程1访问a.write()方法，线程2能访问b.read()方法吗？可以，因为线程1拿到的是锁是 a,而线程2访问b.read()需要的是锁是b。

现在你应该明白了这句话， 对于实例同步方法，锁是当前实例对象。对于静态同步方法，锁是当前对象的Class对象 。

原创文章转载请注明出处：[哪个对象才是锁？]( http://ifeve.com/who-is-lock/comment-page-1/#comment-24444)