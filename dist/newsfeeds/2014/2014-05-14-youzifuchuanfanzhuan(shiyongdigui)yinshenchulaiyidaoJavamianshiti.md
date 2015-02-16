由字符串反转(使用递归)引申出来一道Java面试题
--------

如何面试一个从事编程工作的开发人员既困难又乏味，幸好还有很多值得参考的指南，比如：《Joel Guerilla Guide to interviewing》，但最后雇佣与否，还得由你自己决定。为了快速地了解他们的编程能力，我想到了一个关于字符串反转的问题，有人用这道题取得不错的效果，这道题的答案有很多种，因此这给了你足够的空间去考察候选者的技能，我自己思考了会儿，找到好几种答案如何用Java实现字符串的反转。候选者的答案正好是面试官了解他们如何思考的一种方式。你可以用相关的接口来定义这道题，里面有一个未实现的方法。

```java  
 
public interface Reverser {  
   	public String reverse(String str);   
}   
```` 

在Java中，最好的实现就是用JDK中StringBuffer的反转方法，它不仅速度快，效率高，而且还知道如何处理unicode代理对(surrogate pairs)。其它方案基本上都可以忽略掉。

```java   

public class JdkReverser implements Reverser {   
       public String reverse(String str) {  
            if ((null == str) || (str.length() <= 1)) {
                return str;  
            }   
            return new StringBuffer(str).reverse().toString();  
        }   
}  
```  

不仅选择把趣味性地实现当做一种答案，而且候选者有没有重用JDK，或者告诉你“JDK中有那么些东西是可以去实现的”，哪一种好呢，google一下可以帮你找到JDK的解决方案，你总不希望开发者实现一切。

处理问题：

问他代码中什么地方有bug，即使没有。或者代码怎么会报错，他的答案至少可以引出一个讨论关于如何处理空值的话题来。

 返回null
 返回“”
 抛出NullPointerException
 抛出IllegalArgumentException
 

第二个讨论的焦点是是如何去优化解决方法，像返回字符串本省“”,长度为1的字符串（本省就是反转）

递归（Recursion)：

之后要求应聘者在反转的问题上写一个递归的方案（这至少是漂亮的，但至少可用）

```java   

public String reverse(String str) {  
           if ((null == str) || (str.length()  <= 1)) {  
               return str;  
           }  
           return reverse(str.substring(1)) + str.charAt(0);  
       }   
```  
有些开发人员在脑海中想不到处理递归，或者需要时间和一些提示。那些不能处理递归的很有可能对于复杂的问题没法完成。

你可以问他们关于递归方案的效率，询问尾(Tail)递归，询问 “+”操作的效率，如何处理，关于为什么String都是不可变的（至少在大多时候这么问），反转“Stephan”时，问候选者有多少个字符串对象创建。在讨论中，开发者说“Easy”，他在整个大学都在用Lisp语言，之前我还不知道，现在听起来真是个极好的消息。你还可以询问在上面代码结束递归的停止条件。

更多的方案：

在适当的位置调动StringBuffer：
```java   

public String reverse(String str) {   
            if ((null == str) || (str.length()  <= 1 )) {  
                return str;   
            }  
            StringBuffer result = new StringBuffer(str);  
            for (int i = 0; i < (str.length() / 2); i++) {  
                int swapIndex = str.length() - 1 - i;  
                char swap = result.charAt(swapIndex);  
                result.setCharAt(swapIndex, result.charAt(i));  
                result.setCharAt(i, swap);  
            }   
            return result.toString();  
        }
采用调用数组的方法：

public String reverse(String str) {   
          if ((null == str) || (str.length() <= 1)) {   
              return str;   
          }   
          char[] chars = str.toCharArray();   
          int right = chars.length - 1;   
          for (int left = 0; left < right; left++) {  
              char swap = chars[left];   
              chars[left] = chars[right];  
              chars[right--] = swap;   
          }   
          return new String(chars);  
      }   

SringBuffer追加的方法：

public String reverse(String str) {   
          if ((null == str) || (str.length() <= 1)) {   
              return str;   
          }    
          StringBuffer reverse = new StringBuffer     (str.length());   
          for (int i = str.length() - 1; i >= 0; i--) {
            reverse.append(str.charAt(i));   
          }   
          return reverse.toString();   
      }  
  }   

```  

也许候选人还知道棘手的XOR swapping solution方法。

这是一个开放性的领域，你可以要求候选者写一个JUnit 测试它的反转方法，这样不仅可以展现他写测试单元的能力，而且作为测试用例，他所考虑的条件(“”,null，”A”，奇数长度的字符串，偶数长度的字符串,….）

在你决定是否雇用时，希望以上能帮上你，对自己来说，在将来的某个时候希望同样可以我帮助到自己，就像Joel说的：“疑人不用，用人不疑（when in doubt ,always no hire)。”

 

英文原文：Codemonkeyism  
编译：刘志军   
译文链接： http://www.importnew.com/501.html   