为什么要使用String
----------

最近在培训课期间指导初学者。任务之一就是要大家完成一个类，要求这个类对key为String类型的map执行dwarwle操作。其中一位学员完成的类中，有如下方法：

```java
void dwarwle(HashMap<String,Dwarwable> mapToDwarwle, String dwarwleKey){
    for( final Entry<String, Dwarwable> entry : mapToDwarwle.entrySet()){
        dwarwle(entry.getKey(),entry.getValue(),dwarwleKey);
    }
}
```

这段代码总的来说是OK的。该方法将map中每个Dwarable的key和值，以及和它期望被分解的dwarwleKey一同传得给另一个调用方法。因为功能简单，我就不详细描述了。只要了解dwarwle的含义，就能轻易地知道这个方法会干什么。这样的函数简单且具有较好的可读性。但是，这个方法期待参数是一个HashMap，而不是Map。为什么在这里我们会强迫调用者使用HashMap呢？如果调用者出于某种原因需要使用TreeMap，那么是不是还要重新添加另外一个相同的方法来接受TreeMap呢？ 当然不是。

> “参数类型使用接口，调用时传入实现该接口的对象。”

这位初学者使用Map代替了HashMap。但是大约5分钟之后，这位聪明的女士又提出了这样一个问题：

>“如果我们用Map替换HashMap，那么为什么不用CharSequence来替换String呢？”

突然要回答这样的问题可不是那么容易的。首先我想到是，我们通常都那么做，这就是原因。但是这个答案根本没有说服力，至少我本人不会接受这样的回答，我也希望我的学生不要接受这样的答案。这是一种非常独裁方式的回答。

真正的答案是，因为这个参数作为Map的key，而Map的key通常期望是不可变的（至少变化不会影响equals和hashCode的计算）。CharSequence是一个接口，Java并没有规定接口的可变性，只有具体的实现才能决定。String是CharSequence的具体实现，被广泛熟知并且经过了严格的测试，在这里是个不错的选择。

在这个具体的例子中，我们更倾向于String，因为它是不可变的（Immutable）。并且我们不能完全信任调用者会传递一个不可变的CharSequence的具体实现。假如我们可以信任调用者，那么我们可能为此付出代价。当StringBuilder作为参数传递到该方法，并且之后它的值发生了改变，我们写的类库就很可能不会工作。当设计API或者类库的时候，我们要考虑的不仅是我们期望的某些可能，而且需要考虑现实中的种种可能。

>“实践才是检验真理的唯一标准。”

不仅限于类库，这也可能适用于其他产品。这似乎扯远了。

原文链接： http://www.javacodegeeks.com/2014/10/why-to-use-string.html  
翻译：  nealjob  
译文链接： [importnew](http://www.importnew.com/13664.html)  