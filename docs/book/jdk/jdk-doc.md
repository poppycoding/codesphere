<center>

![Jdk](../../media/jdk.svg ':size=7%')

### <font color=red>Java Doc Best Practice</font> <!-- {docsify-ignore} -->
</center>

!> **Overview**

Javadoc是java编程中很重要的一部分, 同时也是software project成功的因素之一. 完整的javadoc最终以html形式渲染
在浏览器中, 然而实际情况, 大多数情况我们很少会看完整的html页面, 通常是在阅读源码的时候去阅读上面的注释, 所以一个
最佳目标, 就是保持你的javadoc像代码一样可读.

!> **Javadoc Summary**

Javadoc有三中类型: Class Level, Field Level, Method Level.
> Class Level
```java
/**
* Hero is the main entity we'll be using to . . .
* 
* Please see the {@link ... Person} class for true identity
* @author Captain America
* 
*/
public class SuperHero extends Person {
    private String heroName;
}
```

> Field Level 
```java
public class SuperHero extends Person {
    /**
     * The public name of a hero that is common knowledge
     */
    private String heroName;
}
```

> Method Level
```java
public class SuperHero extends Person {
    private String heroName;

    /**
     * <p>This is a simple description of the method. . .
     * <a href="http://www.supermanisthegreatest.com">Superman!</a>
     * </p>
     * @param  incomingDamage the amount of incoming damage
     * @return  the amount of health hero has after attack
     * @see <a href="http://www.link_to_jira/HERO-402">HERO-402</a>
     * @since 1.0
     */
    public int successfullyAttacked(int incomingDamage) {
        // do things ...
        return 0;
    }
}
```

!> **Javadoc More Standards**

**Public和Protected**

所有Public和Protected方法都应当有相应的Javadoc

**使用标准的Javadoc风格注释**

以两个星号开头、以单个星号结尾，并且每行要以星号开头

**用简单的HTML tags就行了，不需要XHTML**

Javadoc用HTML tags 来识别段落、列表等, 同时Javadoc 的 parser 其实会帮你把没闭合的 tags 自动闭合

**用单个p来分割段落**

Javadoc 经常会需要分段, 在两段之间写上一行p就可以了，不需要闭合。
```java
/**
 * First paragraph.
 * <p>
 * Second paragraph.
 * May be on multiple lines.
 * <p>
 * Third paragraph.
 */
public class SuperHero extends Person {
    // ...
}
```

**用单个li来标记列表项**

列表在Javadoc中也很常用，比如用来表示一组选项、一些问题等等。推荐的做法是用一个 <li> 作为每项的开头
```java
/**
 * First paragraph.
 * <p><ul>
 * <li>the first item
 * <li>the second item
 * <li>the third item
 * </ul><p>
 * Second paragraph.
 */
public class SuperHero extends Person {
    // ...
}
```

**首句很重要**

Javadoc的首句是一个总的摘要，在折叠的时候只会显示这一句。因此首句必须是个总结性的描述，最好简洁有力, 同时建议
首句自成一个段落，这让代码看起来更清晰。

对于英文注释，推荐使用第三人称来描述，比如 “Gets the foo”、“Sets the bar”、“Consumes the baz”。避免使用
第二人称，比如 “Get the foo”。

**用 “this” 指代类的对象**

当你想描述这个类的一个实例（对象）的时候，用 “this” 来指代它，比如 “Returns a copy of this foo with the bar value updated”

**别写太长的句子**

尽量让一句话能容纳在一行中，一般来说一行有 80 到 120 个字符。 新的句子就另起一行，这会让代码可读性更好，也会让以后改写 Javadoc 容易很多。

**正确使用 @link 和 @code**

很多地方的描述需要涉及到其他类或方法，这时最好用 @link 和 @code。

@link 会最终变成一个超链接，它有以下几种形式：

```java
/**
 * First paragraph.
 * <p>
 * Link to a class named 'Foo': {@link Foo}.
 * Link to a method 'bar' on a class named 'Foo': {@link Foo#bar}.
 * Link to a method 'baz' on this class: {@link #baz}.
 * Link specifying text of the hyperlink after a space: {@link Foo the Foo class}.
 * Link to a method handling method overload {@link Foo#bar(String,int)}.
 */
public class Foo {
    public void baz(){
        // ...
    }
    public void bar(){
        // ...
    }
    public void bar(String str, int i){
        // ...
    }
}
```

**不要在首句中使用 @link**

之前提到，Javadoc 的首句也被用作概要，首句中的超链接会让读者感到混乱。如果一定要在第一句话中引用其它类或方法，始终用 @code 而不是 @link，第二句开始再用 @link。


**null、true、false 不必用 @code 标记**

null、true、false 这些词在 Javadoc 中太常用了，如果每次都加上 @code，无论是对读者还是作者都是个负担。


**使用 @param、@return 和 @throws**

几乎所有方法都会输入几个参数、输出一个结果，@param 和 @return 就是用来描述这些输入输出参数的，@throws 用于描述方法抛出的异常。
如果有多个输入参数，@param 的顺序也要和参数一致。@return 应当始终放在 @param 之后，然后才是 @throws。


**用短语来描述 @param 和 @return**

@param 和 @return 后面跟的的描述是个短语，而非完整的句子，因此它得用小写字母开头（经常是 the），结尾也不需要用句号。


**用 if-句来描述 @throws**

@throws 通常跟着一个 “if” 句子来描述抛异常的情形，比如 “@throws IllegalArgumentException if the file could not be found”。


**@param 的参数名之后空两格**

在源代码中阅读 Javadoc 的时候，如果参数名后面只有一个空格，读起来会有点困难，两个空格就好很多。另外，避免把参数按列对齐，否则参数改名、增减参数的时候会很麻烦。

```java
public class SuperHero extends Person {
    /**
     * <p>This is a simple description of the method. . .
     * <a href="http://www.supermanisthegreatest.com">Superman!</a>
     * </p>
     * @param  incomingDamage the amount of incoming damage
     * @return  the amount of health hero has after attack
     * @see <a href="http://www.link_to_jira/HERO-402">HERO-402</a>
     * @since 1.0
     */
    public int successfullyAttacked(int incomingDamage) {
        // do things ...
        return 0;
    }
}
```

!> **Javadoc Generation**


















