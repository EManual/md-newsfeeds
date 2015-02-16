由字符串反转(使用递归)引申出来一道Java面试题
---------

### Java是否支持多重继承？

好了，很明显传统意义上Java并没有多继承，所以正确的答案应该是：“没有”或者“有，但是”或者“没有，但是”。单从这点就可以探索出不同的方法来。通常我会问“是否因Java语言的设计者笨到没法实现多重继承”作为我们话题的开始，为什么C++那帮家伙实现了呢？我主要以菱形继承问题来说明：

在面向对象的编程语言中总伴随着多重继承及组织的理解。菱形继承问题就是在如下情景时出现含糊不清的情况：当两个类B和C继承自A，类D同时继承自B和C，如果D中的一个方法调用一个在A中定义的方法(不是覆盖方法).而B和C分别用不同地方式重写（overridden)了方法，那么这个方法到底是继承的哪个类呢，B还是C？


另一探索性的方法是：Java怎么模拟“多重继承”？我想答案已经浮出水面了，就是接口（Interface）。我们经常在Java中讨论接口，那么应试者在什么时候，怎么使用接口的呢？使用接口又有什么好处呢？应试者是否喜欢使用接口？我可以检测他对建模有多熟练，有时还会让他针对接口画画图。我们继续谈论Java中接口的问题，当两个接口有相同的静态域（field)，有一个类实现了Java中所谓的“多继承”的两个接口时会出现什么情况？

```java

public interface I1 {  
   String NAME = "codemonkeyism";  
}  
 
public interface I2 {  
   String NAME = "stephan";  
}  
   
public class C implements I1, I2 {  
   public static void main(String[] args) {  
      System.out.println(NAME);  
   }  
}  

```

Java坚持了他们的做法，语言的设计者决定不让其编译通过。
```xml

C.java:3: reference to NAME is ambiguous, both   variable NAME   
              in I1 and variable NAME in I2 match
      System.out.println(NAME);  
                         ^
1 error  
```

还有更多的方法和我们的应试者去探讨多继承的问题，例如：接口中方法的修饰符是什么？对于菱形继承问题使用traits实现混合结构是否比使用接口更好，还是和多继承一样的糟糕？正如我不在那么钟爱继承一样，大量使用继承是一种代码异味。我们还可以和应试者讨论继承的负面因素—–重耦合的例子。

### 为什么会有这问题？

我为什么会问这个问题呢，从这个问题中又学到了什么？继承在面向对象（OO）中是很基础的概念，是每个Java程序员都应该理解的概念。同时也是他反映到工作中，理解超出语法层面的另一个基本的特性。所以对于多继承问题，我喜欢那些可以引出很多可以探讨与思考的问题。这个继承问题只是多继承、语言设计、代码味道、解决方案、接口、基于角色的开发等众多问题中的一种而已。


英文原文：Codemonkeyism  
编译：ImportNew - 刘志军  
译文链接： [http://www.importnew.com/567.html](http://www.importnew.com/567.html)