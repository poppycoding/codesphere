<center>

![Jdk](../../media/release.png ':size=30%')

### <font color=red>Release Version</font> <!-- {docsify-ignore} -->
</center>

!> **Agile Release Train**

Release Train字面意义就是"火车发布", 就像生活中的车站发车一样, 在软件行业, 项目通常也会定时定点发布. 尤其是对
于复杂的项目, 系统规模越大, 加入的包越多, 就越有可能进入软件管理的领域里的"依赖地狱", 版本混乱导致新版本难以发
布; 而release train就是软件行业一种发布模式, 可以有效保证软件产品的持续迭代, 保证产品发布的稳定及正确性.

例如: 规定每个月的一号为发布日期, 这个时间点就作为各个团队的deadline做好统一的发车准备, 如果中间某一个团队出现意外
不能按点ready, 那就及时下车, 等待下一趟车, 并通知整个项目尽早缩小影响范围.

这种发布模式都会涉及到版本的命名, 虽然命名方式不同, 但是都有一种类似递增的规律:
- IOS 14, IOS 14.1, IOS14.1.1
- macOS Mojave, macOS Sierra
- Spring Cloud Greenwich, Spring Cloud Hoxton

!> **Project Module**

一个大的项目按照release train发布, 内部会涉及不同的模块, 而每一个模块的就是对应一个project module, 以最新版
本Spring Data 2020.0.2版本为例, 内含有多个Project Module模块: 
- Spring Data JDBC 2.1.2
- Spring Data JPA 2.4.2
- Spring Data MongoDB 3.1.2
- Spring Data Redis 2.4.2

!> **Semantic Versioning**

[Semantic Version][1] 定义了一套版本号规则的标准, 语义化版本号: X.Y.Z (主版本号.次版本号.修订号)

当主版本号更新时, 次版本号和补丁号都要归零；次版本号更新时补丁号归零, 递增规则如下: 
- 主版本号: 当你做了不兼容的API修改 (改变很大)
- 次版本号: 当你做了向下兼容的功能性新增 (改变不大)
- 修订号: 当你做了向下兼容的问题修正 (bug修复,代码优化等)

也既是, 修复问题但不影响API时, 递增修订号; API保持向下兼容的新增及修改时, 递增次版本号; 进行不向下兼容的修改时, 
递增主版本号. 版本号均从零开始, 但是当主版本号为零时0.x.y (0.1.0), 代表了软件处于开发初始阶段, 而1.0.0的版本号
则代表了一个稳定的公共API.

!> **Calendar Versioning**

[Calendar Versioning][2] 定义了一套版本号规则的标准, 日历化版本号: MAJOR.MINOR.MICRO-MODIFIER (主要.次要.微小-可选修饰符)

日历化版本不是基于任意数字,而是基于项目发布日期的版本控制约定. Versioning gets better with time!

整体意义与语义化版本号类似:
- 主要: 主要版本号, 版本中的第一个数字
- 次要: 次要版本号, 版本中的第二个数字
- 微小: 版本中的第三个且通常是最终数字
- 修饰符: 可选的文本标记,例如 "dev", "alpha", "beta", "rc1", 依此类推

与语义化版本不同的是, 因为日期格式的多种多样, 衍生出多种多样的日历化版本, 通常规则如下:
- YYYY: 年份全称 - 2006, 2016, 2106
- YY: 年份缩写 - 6, 16, 106
- 0Y: 以零填充的年份 - 06, 16, 106
- MM: 月份缩写 - 1, 2 ... 11, 12
- 0M: 以零填充的月份 - 01, 02 ... 11, 12
- DD: 日 - 1, 2 ... 30, 31
- 0D: 以零填充的日 - 01, 02 ... 30, 31

!> **Spring Release Train**

Spring Release Train版本号都是按照字母表顺序来发布, 如2020之前的版本:

[Spring Cloud][3]

<style>
table th:first-of-type {
    width: 7cm;
}
table th:nth-of-type(2) {
    width: 4cm;
}
</style>

|  Release Train                           | Date    |
|  :----                                   | :----:  |
| Spring Cloud **<font color=red>Angel**   | 2015-06 |
| ...                                      | ...     |
| Spring Cloud **<font color=red>Dalston** | 2017-04 |
| ... ...                                  | ...     |
| Spring Cloud **<font color=red>Hoxton**  | 2019-11 |

[Spring Data][4]

|  Release Train                          | Date    |
|  :----                                  | :----:  |
| Spring Data **<font color=red>Arora**   | 2015-06 |
| ...                                     | ...     |
| Spring Data **<font color=red>Dalston** | 2017-04 |
| ...                                     | ...     |
| Spring Data **<font color=red>Hoxton**  | 2019-11 |


[1]: https://semver.org/ "语义化版本"
[2]: https://calver.org/ "日历化版本"
[3]: https://spring.io/projects/spring-cloud#learn "spring-cloud"
[4]: https://spring.io/projects/spring-data#learn "spring-data"