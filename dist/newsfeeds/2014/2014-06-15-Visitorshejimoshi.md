Visitor设计模式
-------

我猜想许多人都知道访问者设计模式，这种模式在“四人帮”的那本可复用面向对象软件基础的书被描述过。这个模式自身其实一点也不复杂（和以往的其他设计模式一样）。
![img](http://emanual.github.io/md-newsfeeds/img/2014061501.png)

如上图所示：

我知道这个模式很久了，但是我至今都不需要它。Java通过本地方式处理多态：方法被调用时是基于调用这个方法的对象运行时的类型，而是不是基于调用对象编译时的类型。
```java
interface Animal{
     void eat();
}
public class Dog implements Animal {
    public void eat() {
        System.out.println("Gnaws bones");
    }
} 
Animal a = new Dog();
a.eats(); // Prints "Gnaws bones"
```  

然而，以上的方式对于参数类型却无法有效的运行。
```java
  public class Feeder {
    public void feed(Dog d) {
        d.eat();
  }
     public void feed(Cat c) {
        c.eat();
    }
}
 
Feeder feeder = new Feeder();
Object o = new Dog();
feeder.feed(o); // Cannot compile!
```

这个问题被称之为双重派发，因为它既要求被调用的方法既基于调用方法的实例，同时也基于方法的参数类型。而对于参数类型而言，Java不是基于本地化方式来处理。为了能够编译通过，下面的代码是必须的：
```java
if (o instanceof Dog) {
    feeder.feed((Dog) o);
} else if (o instanceof Cat) {
    feeder.feed((Cat) o);
} else {
    throw new RuntimeException("Invalid type");
}
```

随着更多重载方法的出现，情况也会变得更加复杂——方法中出现更多的参数，复杂度也会呈指数级别提高。在维护阶段，添加更多的重载的方法需要阅读所有代码，如果程序填充了太多不必要的代码需就要去更新它。多个参数通过嵌套多个if来实现，这对于维护会变得更加糟糕。访问者模式是一种优雅的方式来解决以上同样的效果，不使用多个if，而使用Animal类中的一个单独的方法来作为解决的代价。
```java
public interface Animal {
    void eat();
    void accept(Visitor v);
}
 
public class Cat {
    public void eat() { ... }
    public void accept(Visitor v) {
        v.visit(this);
    }    
}
 
public class Dog {
    public void eat() { ... }
    public void accept(Visitor v) {
        v.visit(this);
    }
}
 
public class FeederVisitor {
    public void visit(Cat c) {
        new Feeder().feed(c);
    }
    public void visit(Dog d) {
        new Feeder().feed(d);
    }
}
```

### 好处：

* 没有逻辑的评价出现
* 只是在Animal和FeederVisitor之间建立依赖,FeederVisitor中只限于visit方法
* 按照推论，当添加新的Animal子类的时候，Feeder类可以保持不变
* 当添加一个新的Animal子类的时候，FeederVisitor类实现一个额外的方法去处理它即可
* 其他的横切逻辑也可以遵循相同的模式，比如：一个来教动物新把戏的训练特征
* 
对于一些简单的例子使用如此长的代码似乎有杀鸡用宰牛刀的感觉。然而，我的经验教会了我像上面简单的填充代码，当随着项目的发展业务逻辑变负责是致命的。

原文链接： frankel   
翻译： ImportNew.com - 潘 凌霄  
译文链接： http://www.importnew.com/11319.html 