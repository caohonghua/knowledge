---
permalink: /java/basic/object-oriented/
---

## 面向对象

### 三大特性

#### 1. 封装

利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，
只保留一些对外接口使之与外部发生联系。用户无需知道对象内部的细节，但可以通过对象对外提供的接口来访问该对象。

**优点:**

* 减少耦合：可以独立的开发、测试、优化、使用、理解和修改
* 减轻维护的负担：可以更容易被程序员理解、并且可以在调试的时候不影响其他模块
* 有效地调节性能：可以通过剖析确定哪些模块影响了系统的性能
* 提高软件的可重用性
* 降低构建大型系统的风险：即使整个系统不可用，但是这些独立的模块有可能是可用的

以下 Person 类封装 name、gender、age 等属性，外界只能通过 get() 方法获取一个 Person 对象的 name 属性和 gender 属性，而无法获取 age 属性，但是 age 属性可以供 work() 方法使用。

注意到 gender 属性使用 int 数据类型进行存储，封装使得用户注意不到这种实现细节。并且在需要修改 gender 属性使用的数据类型时，也可以在不影响客户端代码的情况下进行。

```java
public class Person {

    private String name;
    private int gender;
    private int age;

    public String getName() {
        return name;
    }

    public String getGender() {
        return gender == 0 ? "man" : "woman";
    }

    public void work() {
        if (18 <= age && age <= 50) {
            System.out.println(name + " is working very hard!");
        } else {
            System.out.println(name + " can't work any more!");
        }
    }
}
```


#### 2. 继承

继承实现了  **IS-A**  关系，例如 Cat 和 Animal 就是一种 IS-A 关系，因此 Cat 可以继承自 Animal，从而获得 Animal 非 private 的属性和方法。

继承应该遵循里氏替换原则，子类对象必须能够替换掉所有父类对象。

Cat可以当做 Animal 来使用，也就是说可以使用 Animal 引用 Cat 对象。父类引用指向子类对象称为 **向上转型** 。

```java
Animal animal = new Cat();
```


#### 3. 多态

多态分为编译时多态和运行时多态：

* 编译时多态主要指方法的重载
* 运行时多态指程序中定义的对象引用所指向的具体类型在运行期间才确定

运行时多态有三个条件：

* 继承
* 覆盖（重写）
* 向上转型

下面的代码中，乐器类(Instrument)有两个子类: Wind 和 Percussion，它们都覆盖了父类的 play() 方法，
并且在 main() 方法中使用父类 Instrument 来引用 Wind 和 Percussion 对象。在 Instrument 引用调用 play() 方法时，
会执行实际引用对象所在类的 play() 方法，而不是 Instrument 类的方法。

```java
public class Instrument {
    public void play() {
        System.out.println("Instument is playing...");
    }
}

public class Wind extends Instrument {
    public void play() {
        System.out.println("Wind is playing...");
    }
}

public class Percussion extends Instrument {
    public void play() {
        System.out.println("Percussion is playing...");
    }
}

public class Music {
    public static void main(String[] args) {
        List<Instrument> instruments = new ArrayList<>();
        instruments.add(new Wind());
        instruments.add(new Percussion());
        for(Instrument instrument : instruments) {
            instrument.play();
        }
    }
}

```

### 类图

1. 泛化关系（Generalization）
用来描述继承关系，在Java中使用extends关键字。
![generalization](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/generalization.png)

2. 实现关系(Realization)
用来实现一个借口，在Java中使用implement关键字。
![realization](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/realization.png)

3. 聚合关系(Aggregation)
表示整体由部分组成，但是整体和部分不是强依赖的，整体不存在了部分还是会存在。
![aggregation](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/aggregation.png)

4. 组合关系(Composition)
和聚合不同，组合中整体和部分是强依赖的，整体不存在了部分也不存在了。比如公司和部门，公司没了部分就不存在了。但是公司和员工就属于聚合关系，因为公司没了员工还在。
![composition](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/composition.png)

5. 关联关系(Association)
表示不同对象之间有关联，这是一种静态关系，与运行过程的状态无关，在最开始就可以确定。因此也可以用1对1、多对1、多对多这种关联关系来表示。比如学生和学校就是一种关联关系，一个学校可以有很多学生，但是一个学生只属于一个学校，因此这是一种多对一的关系，在运行开始之前就可以确定。
![association](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/association.png)

6. 依赖关系(Dependency)
和关系关系不同，依赖关系是在运行过程中起作用的。A类和B类是依赖关系主要有三种形式：

* A类是B类中的（某方法的）局部变量；
* A类是B类方法当中的一个参数；
* A类向B类发送消息，从而影响B类发生变化；
![dependency](https://caohonghua.github.io/knowledge/assets/images/java/basic/object-oriented/dependency.png)