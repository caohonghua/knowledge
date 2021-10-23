---
permalink: /quliaty/unit-test-idea/
---

## 单元测试 - IDEA下单元测试详解

> 工欲善其事必先利其器，我们在写单元测试一定要使用工具，这将能大幅度提升编码的效率。本文以IDEA为例，看看如何利用插件提升效率

* 场景准备
* 插件使用
    * 自动生成单元测试
    * 并行测试
    * 代码覆盖率
    * Profile

### 场景准备

准备一个待测试的类, 其中还包含着错误

```java
package tech.pdai.junit4.module;

public class Calculator {

    public int result = 0;

    /**
     * add.
     *
     * @param operand1 first param
     * @param operand2 second param
     * @return sum
     */
    public int add(int operand1, int operand2) {
        result = operand1 + operand2;
        return result;
    }

    public int subtract(int operand1, int operand2) {
        result = operand1 - operand2;
        return result;
    }

    public int multiple(int operand1, int operand2) {
        result = operand1 * operand2;
        for (; ; ) {                    //死循环
        }
    }

    public int divide(int operand1, int operand2) {
        result = operand1 / 0;
        return result;
    }

    public int getResult() {
        return this.result;
    }

}
```

### 插件使用

#### 自动生成单元测试

第一个插件，首推的是JunitGeneratorV2.0

![dev-ut-idea-1.png](/knowledge/assets/images/quality/dev-ut-idea-1.png)

设置默认采用Junit4

![dev-ut-idea-2.png](/knowledge/assets/images/quality/dev-ut-idea-2.png)

如有必要可以设置生成的模板

![dev-ut-idea-3.png](/knowledge/assets/images/quality/dev-ut-idea-3.png)

测试下

![dev-ut-idea-4.png](/knowledge/assets/images/quality/dev-ut-idea-4.png)

![dev-ut-idea-5.png](/knowledge/assets/images/quality/dev-ut-idea-5.png)

生成单元测试

![dev-ut-idea-6.png](/knowledge/assets/images/quality/dev-ut-idea-6.png)

补充完整代码

```java
package tech.pdai.junit4.module;

import org.junit.After;
import org.junit.Before;
import org.junit.Ignore;
import org.junit.Test;

import static org.junit.Assert.*;

public class CalculatorTest {

    private static Calculator cal=new Calculator();

    @Before
    public void setUp() throws Exception {
        System.out.println("before");
    }

    @After
    public void tearDown() throws Exception {
        System.out.println("after");
    }

    @Test
    public void add() {
        cal.add(2,2);
        assertEquals(4,cal.getResult());
    }

    @Test
    public void subtract() {
        cal.subtract(4,2);
        assertEquals(2,cal.getResult());
    }

    @Ignore
    public void multiply() {
        fail("Not yet implemented");
    }

    @Test(timeout = 2000)
    public void divide() {
        for(;;);
    }

    @Test(expected = ArithmeticException.class)
    public void testDivideByZero(){
        cal.divide(4,0);
    }

}
```

执行结果

![dev-ut-idea-7.png](/knowledge/assets/images/quality/dev-ut-idea-7.png)

### 并行测试

在大量的单元测试时，如何提升测试的效率呢？肯定是并行，所以你可以用如下的插件

![dev-ut-idea-8.png](/knowledge/assets/images/quality/dev-ut-idea-8.png)

看下相关测试触发按钮和输出：

![dev-ut-idea-9.png](/knowledge/assets/images/quality/dev-ut-idea-9.png)

### 代码覆盖率

如何快速看本地代码测试覆盖率呢？

![dev-ut-idea-10.png](/knowledge/assets/images/quality/dev-ut-idea-10.png)

代码覆盖率

![dev-ut-idea-11.png](/knowledge/assets/images/quality/dev-ut-idea-11.png)

![dev-ut-idea-12.png](/knowledge/assets/images/quality/dev-ut-idea-12.png)

### Profile

* CPU Profile

Flame Graph

![dev-ut-idea-13.png](/knowledge/assets/images/quality/dev-ut-idea-13.png)

Call Tree

![dev-ut-idea-14.png](/knowledge/assets/images/quality/dev-ut-idea-14.png)

Method List

![dev-ut-idea-15.png](/knowledge/assets/images/quality/dev-ut-idea-15.png)

* Allocation Profile

![dev-ut-idea-16.png](/knowledge/assets/images/quality/dev-ut-idea-16.png)
