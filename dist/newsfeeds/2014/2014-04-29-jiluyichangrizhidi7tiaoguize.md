记录异常日志的7条规则
------------------

最近一直在帮忙调试一些让人头大的bug问题，这才意识到如何记录异常日志对于简化调试的重要性，我总结了几点记录异常日志的最佳实践发表在此。

## 1、记录技术性异常而不是用户异常 

用户异常（如：“登录用户名已经存在”）除了显示给用户，要么什么都别管，要么根本就不是异常（“用户尚未认证”）。技术性异常（如：“文件存储不够，没法订阅此产品”）才是你需要调试而为此做出反应的，如果你记录所有事情很有可能因日志实体太长而不能真正有意义的反映到你所记录异常日志中。你应该查明日志文件中的每个异常找到其原因（“是bug吗”），过多的异常将使你草率地对待异常（“额，仅仅是一个普通异常”）。

## 2、把数据存储在异常中以便更易于记录

“没法收取账户费用“这样的异常你应该存储异常的上下文,就像Junit所做的（“期望…但是得到…”）以便更易于调试。

CannotChargeMoneyAccountExceptionMoney（moneyInAccount, Money toCharge, Account account）这段子句可以理解为：“尝试收取账户1234567890 20EUR，但是只有10EUR可用”相对于“扣除失败”，这使得在之后有意义的方式记录异常变得更简单。

## 3、记录异常的描述信息

糟糕的例子来自于Sun：ClassCastException不能显示什么类你想要转换。 现在甚至可以发现：   
```java
final String s = "Hello";
final int x = (Integer) s;
```  

在编译时就告诉你：  
```console 
 T.java:4: inconvertible types  
 found   : java.lang.String  
 required: java.lang.Integer  
          final Integer x = (Integer) s;  
```  

在运行期间Java抛出的异常
```console 
  Exception in thread "main" java.lang.ClassCastException:  
  java.lang.String cannot be cast to java.lang.Integer
``` 

这样显得比前者要好

## 4、输出导致异常的原因

如果在异常中有一个包装的方式作为异常的起因，那么记录所有的原因。一些日志框架已经帮你这样做了，有些还没有，因此要确保异常的所有原因记录在日志文件中，保证所有相关的堆栈追踪信息的开始处记录在你的文件中，而不是杂乱无章的。

## 5、选择合理的日志级别

如果你的异常危急的，记录它为Level.CRITICAL，如果你需要确定危急对你来说意味着什么（通常意味着金钱损失），例如如果一个预定功能失效，或者由于技术问题用户不能注册，然后你就有一个危急问题需要解决。 监控日志文件中危急的异常，因为你正在蒙受着损失。拥有你自己的异常实现isCritical()或者CRITICALException接口，用封装类以及正确的日志级别记录异常到文件中。如果你的日志框架找不到合理的级别，那么创建它。

## 6、不要记录后又重新向外抛出

记录日志后重新抛出异常是一个异常反模式(anti-pattern)。  
```java  
catch (NoUserException e) {
       LOG.error("No user available", e);
       throw new UserServiceException("Nouseravailable", e);
}
```
不要这么做，你的日志文件包含相同的异常好几次在堆栈级别上，记住仅仅记录一次。

## 7、不要使用System.out或者System.err记录

使用日志框架，它可以比你处理得更好

希望这些规则在处理异常日志时能帮助到你，能使你更容易调试。


英文原文：Codemonkeyism，编译：ImportNew - 刘志军  
译文链接： http://www.importnew.com/518.html