---
permalink: /quality/sonarlint/
---

## 代码质量 - 代码质量管理: Sonarlint插件详解

> 在提交代码前，为提升代码质量还需要使用一些静态代码工具检查代码质量，最为常用的是Sonar；在本地IDE中可以配置Sonarlint插件进行检查

* Sonar是什么？
* Sonarlint的使用

### Sonar是什么？

> Sonar是一个用于代码质量管理的开源平台，用于管理源代码的质量 通过插件形式，可以支持包括java,C#,C/C++,PL/SQL,Cobol,JavaScrip,Groovy等等二十几种编程语言的代码质量管理与检测

Sonar可以从以下七个维度检测代码质量，而作为开发人员至少需要处理前5种代码质量问题

* **不遵循代码标准** sonar可以通过PMD,CheckStyle,Findbugs等等代码规则检测工具规范代码编写
* **潜在的缺陷** sonar可以通过PMD,CheckStyle,Findbugs等等代码规则检测工具检测出潜在的缺陷
* **糟糕的复杂度分布** 文件、类、方法等，如果复杂度过高将难以改变，这会使得开发人员难以理解它们 且如果没有自动化的单元测试，对于程序中的任何组件的改变都将可能导致需要全面的回归测试
* **重复** 显然程序中包含大量复制粘贴的代码是质量低下的，sonar可以展示源码中重复严重的地方
* **注释不足或者过多** 没有注释将使代码可读性变差，特别是当不可避免地出现人员变动时，程序的可读性将大幅下降 而过多的注释又会使得开发人员将精力过多地花费在阅读注释上，亦违背初衷
* **缺乏单元测试** sonar可以很方便地统计并展示单元测试覆盖率
* **糟糕的设计**  通过sonar可以找出循环，展示包与包、类与类之间相互依赖关系，可以检测自定义的架构规则 通过sonar可以管理第三方的jar包，可以利用LCOM4检测单个任务规则的应用情况， 检测耦合。

官方网站： https://www.sonarsource.com/products/sonarlint/

### Sonarlint的使用

> 我以IDEA环境下做Demo和测试。

* 安装插件

![dev-qt-sonarlint-1.png](/knowledge/assets/images/quality/dev-qt-sonarlint-1.png)

* 配置插件

![dev-qt-sonarlint-3.png](/knowledge/assets/images/quality/dev-qt-sonarlint-3.png)

除此此外还可以配置链接到Sonarqube服务器（当远程SonarQube有更新,可以点击下面的按钮Update Binding进行更新；这样可以保障团队成员使用的和最后CI环境是一套规则， 这是特别有用的，因为我们可能在服务器上禁用掉一些特例规则，然后我们可以通过这种方式同步给所有开发成员）

![dev-qt-sonarlint-2.png](/knowledge/assets/images/quality/dev-qt-sonarlint-2.png)

* 扫描代码

写一个小Demo

```java
/**
 * This class is for sonarlint util test.
 *
 * @author pdai
 * @version 2021/1/29 下午8:25
 */
public class TestUtil {

    /**
     * sum.
     *
     * @param a a
     * @param b b
     * @return sum
     */
    public static int add(int a, int b) {
        int c = a + b;
        return c;
    }

}
```

检查

![dev-qt-sonarlint-4.png](/knowledge/assets/images/quality/dev-qt-sonarlint-4.png)

**SonarLint插件在IntelliJ IDEA 中页面显示的名词解释：**

* Current file：当前文件（展示当前文件存在的哪些问题，以及解决方法）
* Rule：规则（告诉你问题的具体内容，并在最后告诉你如何解决这个问题）
* Report：报告（鼠标移动到项目根目录，右键选择Ayalyze，选择Ayalyze with SonarLint，完成之后的结果显示，包含整个项目的所有文件的问题）
* Locations：位置（告诉你，在哪个地方有问题，问题的重复位置）
* Issues：问题
    * 每个issue有五种等级
        * BLOCKER（致命）:会影响应用程序的缺陷：内存泄漏，未关闭的JDBC连接…必须立刻修复的代码；
        * CRITICAL（关键 ）：可能会影响应用程序的缺陷或者是安全性缺陷：空的catch块，sql注入，…必须立刻查看代码；
        * MAJOR（主要）：可能会影响开发者效率的质量缺陷：未覆盖的代码，重复块，未使用的参数….
        * MINOR（微小）：可能会影响开发者效率的质量缺陷：每行不能太长，“switch”语句应该至少有三个条件，….
        * INFO（未知）：既不是缺陷也不是质量问题，只是一个发现。
    * 每个issue有三个状态
        * Bug：窃听器（可靠性），避免错误和未定义的行为
        * Vulnerability：漏洞（安全），避免破坏或攻击
        * Code Smell：代码嗅觉（可维护性），简化代码更新，并提高开发人员的速度

根据检查提示，修复后

![dev-qt-sonarlint-5.png](/knowledge/assets/images/quality/dev-qt-sonarlint-5.png)

