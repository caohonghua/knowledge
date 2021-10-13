---
permalink: /design/factory-method/
---

## 创建型 - 工厂方法(Factory Method)

> 本文主要分析设计模式 - 工厂方法(Factory Method)，它定义了一个创建对象的接口，但由子类决定要实例化哪个类。工厂方法把实例化操作推迟到子类

* 意图
* 类图
* 实现
* JDK
* 总结

### 意图

定义了一个创建对象的接口，但由子类决定要实例化哪个类。工厂方法把实例化操作推迟到子类

### 类图

在简单工厂中，创建对象的是另一个类，而在工厂方法中，是由子类来创建对象

下图中，Factory 有一个 doSomething() 方法，这个方法需要用到一个产品对象，这个产品对象由 factoryMethod() 方法创建。该方法是抽象的，需要由子类去实现

![factory-method-1](/knowledge/assets/images/design/factory-method_1.png)

### 实现

```java
public abstract class Factory {
    abstract public Product factoryMethod();
    public void doSomething() {
        Product product = factoryMethod();
        // do something with the product
    }
}
```

```java
public class ConcreteFactory extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct();
    }
}
```

```java
public class ConcreteFactory1 extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct1();
    }
}
```

```java
public class ConcreteFactory2 extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct2();
    }
}
```

### JDK

* [java.util.Calendar]
* [java.util.ResourceBundle]
* [java.text.NumberFormat]
* [java.nio.charset.Charset]
* [java.net.URLStreamHandlerFactory]
* [java.util.EnumSet]
* [javax.xml.bind.JAXBContext]


### 总结

> 知识点的东西在上面参考文章中写的非常详细，读完之后问自己下面几个问题，直到可以流利的回答

* 使用的场景?

* 本质是什么?

* 它解决了什么问题?

* 它体现了设计模式中什么原则?

* 存在的缺陷?

* 你认为与它相关的设计模式有哪些? 它们之间的区别有哪些?

* 开源架构中哪些使用了这一模式?