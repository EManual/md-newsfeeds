得墨忒耳定律
-----------

## 前言

这篇文章中，我想谈一谈得墨忒耳定律（Law of Demeter，缩写LoD）。我觉得这个主题对于写出一手简洁、精心设计和易于维护的代码，是极其重要的。

根据我的经验，看到这条规则被打破，往往意味着我可以嗅到一股糟糕的设计在其中。而依据此定律去编写/重构，会使代码改善，清晰易懂且更易于维护。

## 得墨忒耳定律是什么?

我们从下面这几条基本规则开始:

得墨忒耳定律－－对象 O 的 M 方法，可以访问/调用如下的：

1. 对象 O 本身
2. M 方法的传入参数
3. M 方法中创建或实例化的任意对象
4. 对象 O 直接的组件对象
5. 在M范围内，可被O访问的全局变量

>（译者注: 上述5条是根据英文版维基百科，修正后的更为更准确的说法。与原博文所述略有区别。）

这些都是很简单的规则。

换言之：每个单元（对象或方法）应当对其他单元只拥有有限的了解。

## 一些比喻

最常见的比喻是：不要和陌生人说话

看看这个：假设我在便利店购物。付款时，我是应该将钱包交给收银员，让她打开并取出钱？还是我直接将钱递给她？

再做一个比喻：人可以命令一条狗行走（walk），但是不应该直接指挥狗的腿行走，应该由狗去指挥控制它的腿如何行走。

## 为什么要遵循这个规则？

* 我们可以更改一个类，而无需因连锁反应再去改许多其他的（类）。
* 我们可以改变调用的方法，而无需改变其他任何东西。
* 遵从LOD，让测试更容易被构建。我们不必为了模拟而写很多的’when’和各种return。
* 提高了封装和抽象（下文将举例说明）。
* 基本上，我们隐藏了“xx是如何工作的”。
* 让代码更少的耦合。主叫方法只耦合一个对象，而并非所有的内部依赖。
它通常会更好地模拟现实世界。想想钱包与付款的那个比喻。

## 数数那些“.”?

虽然在代码中充斥着许多“.”意味着Lod定律被违反了，但有时“合并这些点”并没有任何意义。比如我们把下面这样代码:  
```java  
  	getEmployee().getChildren().getBirthdays();
```  
重构成这样子:  
```java 
	getEmployeeChildrenBirthdays()
```   
这样真的好么，我不确定。

## 太多包装类

这是不遵从LOD的另一个结果。在这种情况下，我坚信这类的设计需要被重新处理。

所以，在编码、清理或重构的过程中，我们要遵循某些常识性的规律。

## 一个范例

假设有一个Item类，它包含多个属性。每个属性都有一个名称和值。（这是一个多值属性）

那最简单的实现就是使用Map。
```java    
public class Item {
  private final Map<String, Set<String>> attributes;
 
  public Item(Map<String, Set<String>> attributes) {
    this.attributes = attributes;
  }
   
  public Map<String, Set<String>> getAttributes() {
    return attributes;
  }
}
```    

现在，有一个ItemsSaver类，将使用到Item和其属性：    
 
```java   
public class ItemSaver {
  private String valueToSave;
  public ItemSaver(String valueToSave) {
    this.valueToSave = valueToSave;
  }
 
  public void doSomething(String attributeName, Item item) {
    Set<String> attributeValues = item.getAttributes().get(attributeName);
    for (String value : attributeValues) {
      if (value.equals(valueToSave)) {
        doSomethingElse();
      }
    }
  }
 
  private void doSomethingElse() {
  }
}
```

我想获取某一个具体属性的时候：   
```java   
	Set<String> attributeValues = item.getAttributes().get(attributeName);
	String singleValue = attributeValues.iterator().next();  
	// String singleValue = item.getAttributes().get(attributeName).iterator().next();

```    

很明显，我们遇到一个问题。每当使用Item类的属性时，我们知道了它如何工作，它的内部实现。同时，这也让测试代码难以维护。

看一下这个测试用例(测试框架 Mockito)：你可以想象到这是多么难以变更和维护。

```java  
	Item item = mock(Item.class);
	Map<String, Set<String>> attributes = mock(Map.class);
	Set<String> values = mock(Set.class);
	Iterator<String> iterator = mock(Iterator.class);
	when(iterator.next()).thenReturn("the single value");
	when(values.iterator()).thenReturn(iterator);
	when(attributes.containsKey("the-key")).thenReturn(true);
	when(attributes.get("the-key")).thenReturn(values);
	when(item.getAttributes()).thenReturn(attributes);	
```

可以用真正的Item替代模拟的，但这仍需要创建大量的测试前数据。

让我们来回顾一下：

* 我们暴漏了内部实现－－Item类怎样保存它的属性
* 为了使用属性，我们需要从item对象中拿到属性和它的内部实现相关对象（如测试代码* 中的Set集合values）。
* 如果想改变属性的实现，我们需要更改所有使用Item和其属性的类。这很可能波及多个类。
* 构建的测试繁琐、易错，且需要很多维护。

## 改进

第一个改进是，把对属性的各种操作，委托给Item类本身。

```java    
public class Item {
  private final Map<String, Set<String>> attributes;
  public Item(Map<String, Set<String>> attributes) {
    this.attributes = attributes;
  }
 
  public boolean attributeExists(String attributeName) {
    return attributes.containsKey(attributeName);
  }
 
  public Set<String> values(String attributeName) {
    return attributes.get(attributeName);
  }
 
  public String getSingleValue(String attributeName) {
    return values(attributeName).iterator().next();
  }
}
```  

这样，测试便容易多了：
```java  
	Item item = mock(Item.class);
	when(item.getSingleValue("the-key")).thenReturn("the single value");  
```

我们几乎将属性相关操作的实现都隐藏了。

使用到Item的类，并不知道其内部实现。除了以下两个情形：

1. Item本身仍知道属性被怎样构建。
2. 创建Item的类，也知道属性怎样被实现。  

以上两点意味着，如果我们改变属性的实现（比如变更为不使用map），至少两个其他的类将需要改变。这是高耦合的一个好例子。

## 进一步改进

上面的解决方案有时候（通常？）足够。作为务实的程序员，我们需要知道何时停止。然而，来看如何进一步改进第一种方案。

创建一个Attributes类：  
```java  
public class Attributes {
  private final Map<String, Set<String>> attributes;
 
  public Attributes() {
    this.attributes = new HashMap<>();
  }
 
  public boolean attributeExists(String attributeName) {
    return attributes.containsKey(attributeName);
  }
 
  public Set<String> values(String attributeName) {
    return attributes.get(attributeName);
  }
   
  public String getSingleValue(String attributeName) {
    return values(attributeName).iterator().next();
  }
 
  public Attributes addAttribute(String attributeName, Collection<String> values) {
    this.attributes.put(attributeName, new HashSet<>(values));
    return this;
  }
}

```

然后Item类这样去用它：  
```java  
public class Item {
  private final Attributes attributes;
 
  public Item(Attributes attributes) {
    this.attributes = attributes;
  }
   
  public boolean attributeExists(String attributeName) {
    return attributes.attributeExists(attributeName);
  }
   
  public Set<String> values(String attributeName) {
    return attributes.values(attributeName);
  }
 
  public String getSingleValue(String attributeName) {
    return attributes.getSingleValue(attributeName);
  }
}

```

（注意到了么？Item内属性项的实现被改变，但测试代码无需变更。这是由前面委托的更改导致的。）

在第二种解决方案中，我们改进了属性的封装。现在，连Item类自己都不知道它(属性)是如何工作的。

现在，我们可以变更属性的实现，而不必修改任何一个其他类。而且可以将属性变为如下任一类实现方式：

* 用Set集合保存一组Value
* 用List列表保存一组Value
* 用你能想到的某种完全不同的数据结构
* 只要所有测试都通过，就一切OK。

## 我们获得了什么？  
* 代码更容易维护。
* 测试用例更简单且易于维护。
* 代码更加灵活。我们可以随意更改属性项的实现(map, set, list或其他)
* 属性项的修改，不会影响其他代码。甚至不会影响直接使用它(的代码)。
* 模块化和代码重用。可以在其他代码中重用Attributes类。

原文链接： javacodegeeks     
翻译： ImportNew.com - dust_jead    
译文链接： http://www.importnew.com/10501.html  