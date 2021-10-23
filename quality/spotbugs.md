---
permalink: /quality/spotbugs/
---

## 代码质量 - 代码质量管理: SpotBugs插件详解

> SpotBugs是Findbugs的继任者（Findbugs已经于2016年后不再维护），用于对代码进行静态分析，查找相关的漏洞；在本地IDE中可以配置SpotBugs插件进行检查

* SpotBugs
* IDEA 中使用SpotBug插件
* SpotBugs 额外插件

### SpotBugs

> SpotBugs是Findbugs的继任者（Findbugs已经于2016年后不再维护），用于对代码进行静态分析，查找相关的漏洞; 它是一款自由软件，按照GNU Lesser General Public License 的条款发布。

与查看源代码的Java编译器不同，静态分析会检查Java字节码（已编译的.class文件）是否存在错误模式。错误模式是一种经常容易出错的代码表达式/习惯用法。错误模式的产生可能有多种原因，其中包括：

* 错误使用编程语言的某些特性
* 误用的API方法
* 在维护期间修改代码时误解变量
* 错别字，使用错误的运算符等

在使用之前，我们需要先看下官网的简单介绍，特别是注意点

![dev-qt-spotbugs-1.png](/knowledge/assets/images/quality/dev-qt-spotbugs-1.png)

然后我们看下它提供的哪些检查

![dev-qt-spotbugs-2.png](/knowledge/assets/images/quality/dev-qt-spotbugs-2.png)

* Bad practice 不佳实践：常见代码错误，用于静态代码检查时进行缺陷模式匹配(如重写equals但没重写 hashCode，或相反情况等)
* Correctness 可能导致错误的代码(如空指针引用、无限循环等)
* Experimental 实验性
* Internationalization 国际化相关问题（如错误的字符串转换等）
* Malicious code vulnerability 可能受到的恶意攻击（如访问权限修饰符的定义等）
* Multithreaded correctness 多线程的正确性（如多线程编程时常见的同步，线程调度问题等）
* BogusMultithreaded correctness 多线程的正确性（如多线程编程时常见的同步，线程调度问题等）
* Performance 运行时性能问题（如由变量定义，方法调用导致的代码低效问题等）
* Security 安全问题（如HTTP，SQL，DB等）
* Dodgy code 导致自身错误的代码（如未确认的强制转换、冗余的空值检查等）

### IDEA 中使用SpotBug插件

> IntelliJ SpotBugs插件提供静态字节代码分析，以从IntelliJ IDEA中查找Java代码中的错误。SpotBugs是Java的缺陷检测工具，它使用静态分析来查找 400 多种错误模式，例如空指针取消引用，无限递归循环，对Java库的错误使用和死锁。SpotBugs可以识别大型应用程序中的数百个严重缺陷（通常每 1000-2000 行非注释源语句中大约有 1 个缺陷）

* 下载插件

![dev-qt-spotbugs-3.png](/knowledge/assets/images/quality/dev-qt-spotbugs-3.png)

PS: FindBugs-IDEA的官方信息最后一次更新是在2016年10月17日，显然，这个不能用了，这也是为什么在Intellij的插件库中搜索关键词findbugs和BUG不显示改插件的原因。

* 扫描代码

![dev-qt-spotbugs-6.png](/knowledge/assets/images/quality/dev-qt-spotbugs-6.png)

* 审查相关结果，针对修改

![dev-qt-spotbugs-5.png](/knowledge/assets/images/quality/dev-qt-spotbugs-5.png)

### SpotBugs 额外插件

> SpotBugs 还有对应的额外插件，用于扩展对应的规则，探测出更多的代码问题

* fb-contrib 其中因 FindBugs分叉，SpotBugs需要使用-sb版本。

官网 : https://github.com/mebigfatguy/fb-contrib

* find-sec-bugs

针对安全（Security）增加了大致70+条规则

官网: https://github.com/find-sec-bugs/find-sec-bugs

