- [Learn Microservice](book/geek-time/learn-microservice/doc.md)

- Design Pattern
    - [Strategy](book/desgin-pattern/Strategy.md)

- Spring for All
    - [Spring Framework](book/spring-for-all/spring-framework/guide.md)
    - [Spring Boot](book/spring-for-all/spring-boot/guide.md)
    - [Spring Cloud](book/spring-for-all/spring-cloud/guide.md)

- Junit
    - [Junit5](book/junit/Junit5.md)


<center>

## _<font color=red>Hoist the Colours!</font>_

### _<font color=green>Go and brave the weird and haunted shores at world's end.</font>_

</center>

<u>下划线</u>

![logo](https://docsify.js.org/_media/icon.svg ':size=5%')

!> 一段重要的内容,可以和其他 **Markdown** 语法混用

?> **<font color=red>JDK Beta(1995)</font>**

```java
public class Test{
public static void main(String[]args) {
 }
}
```

```bash
echo "hello"
```

?> _TODO_ 完善示例


- [ ] foo
- bar
- [x] baz
- [] bam <~ not working
    - [ ] bim
    - [ ] lim


### 你好,世界！ :id=hello-world <!-- {docsify-ignore-all} -->


This is a regular paragraph.

<table>
    <tr>
        <td>Foo</td>
        <td>Foo</td>
    </tr>
    <tr>
        <td>Foo</td>
        <td>Foo</td>
    </tr>
</table>

This is another regular paragraph.


|  表头   | 表头  |
|  ----   | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |

| 左对齐 | 右对齐 | 居中对齐 |
|:----  |  ---:  | :---: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |


| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

1.git pull origin master --allow-unrelated-histories

2.git branch --set-upstream-to=origin/master master

<style>
table th:first-of-type {
    width: 4cm;
}
table th:nth-of-type(2) {
    width: 150pt;
}
table th:nth-of-type(3) {
    width: 8em;
}
</style>

| a | b | c |
|---|---|---|
| 列宽 = 3 cm| 列宽 = 5 cm| 列宽 = 8em |

&copy;

I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

[1]: http://google.com/        "Google"
[2]: http://search.yahoo.com/  "Yahoo Search"
[3]: http://search.msn.com/    "MSN Search"


居中:
<center>月是故乡明</center>
左对齐:
<p align="left">月是故乡明</p>
右对齐:
<p align="right">月是故乡明</p>

<font face="黑体" size=2>我是黑体2号字</font>
<font face="黑体" size=3>我是黑体3号字</font>

用爱劈开黑暗,像一道曙光,
<hr/>
是非真假无法判断,不如坚强.
<hr/>


*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

<details>
<summary>展开/收起</summary>
被折叠的内容
</details>

<details>
<summary>展开查看</summary>
<pre><code>
System.out.println("Hello to see U!");
</code></pre>
</details>

- 1
  - 3
* 2
  * 4
* 3


<center>

![logo](../../../media/software-design/design-principle/solid.png ':size=20%')

### <font color=red>Software Design Principles</font> <!-- {docsify-ignore} -->
</center>

!> **单一职责 (Single Responsibility Principle)**: 一个类应该负责自己对象的职责, 避免嵌入其他逻辑

!> **开闭原则 (Open Closed Principle)**: 一个软件实体应当对扩展开放, 对修改关闭

!> **里氏替换原则  (Liskov Substitution Principle)**: 子类型必须能够替换它们的基类型, 反地来的替换不成立

!> **接口隔离原则 (Interface Segregation Principle)**: 使用多个专门的接口而不是使用一个的总接口

!> **依赖倒置原则 (Dependence Inversion Principle)**: 具体要依赖于抽象, 抽象不要依赖于具体

!> **组合复用原则 (Composite / Aggregate Reuse Principle)**:
在一个新的对象里面使用一些已有的对象, 使之成为新对象的一部分, 新对象通过向这些对象的委派达到复用已有功能

!> **迪米特法则 (Law of Demeter) / 最少知识原则 (Least Knowledge Principle)**:
一个对象应当对其它对象有尽可能少地了解, 不要和陌生人说话