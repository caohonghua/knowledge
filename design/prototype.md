---
permalink: /design/prototype/
---

## 创建型 - 原型模式(Prototype)

本文主要分析设计模式 - 原型模式(Prototype)，使用原型实例指定要创建对象的类型，通过复制这个原型来创建新对象

* 意图
* 类图
* 实现
* JDK

### 意图

使用原型实例指定要创建对象的类型，通过复制这个原型来创建新对象。

### 类图

![prototype](/knowledge/assets/images/design/prototype.png)

### 实现

```java
public abstract class Prototype {
    abstract Prototype myClone();
}
```

```java
public class ConcretePrototype extends Prototype {

    private String filed;

    public ConcretePrototype(String filed) {
        this.filed = filed;
    }

    @Override
    Prototype myClone() {
        return new ConcretePrototype(filed);
    }

    @Override
    public String toString() {
        return filed;
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        Prototype prototype = new ConcretePrototype("abc");
        Prototype clone = prototype.myClone();
        System.out.println(clone.toString());
    }
}
```

```html
abc
```

### JDK

* [java.lang.Object#clone()]

