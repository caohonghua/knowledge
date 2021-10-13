---
permalink: /design/adapter/
---

## 结构型 - 适配器(Adapter)

> 适配器模式(Adapter pattern): 将一个类的接口, 转换成客户期望的另一个接口。 适配器让原本接口不兼容的类可以合作无间。 对象适配器使用组合, 类适配器使用多重继承

* 意图
* 类图
* 实现
* JDK

### 意图

把一个类接口转换成另一个用户需要的接口

![adapter-1](/knowledge/assets/images/design/adapter-1.png)

### 类图

![adapter-2](/knowledge/assets/images/design/adapter-2.png)

### 实现

鸭子(Duck)和火鸡(Turkey)拥有不同的叫声，Duck 的叫声调用 quack() 方法，而 Turkey 调用 gobble() 方法。

要求将 Turkey 的 gobble() 方法适配成 Duck 的 quack() 方法，从而让火鸡冒充鸭子！

```java
public interface Duck {
    void quack();
}
```

```java
public interface Turkey {
    void gobble();
}
```

```java
public class WildTurkey implements Turkey {
    @Override
    public void gobble() {
        System.out.println("gobble!");
    }
}
```

```java
public class TurkeyAdapter implements Duck {
    Turkey turkey;

    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }

    @Override
    public void quack() {
        turkey.gobble();
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        Turkey turkey = new WildTurkey();
        Duck duck = new TurkeyAdapter(turkey);
        duck.quack();
    }
}
```

### JDK

* [java.util.Arrays#asList()]
* [java.util.Collections#list()]
* [java.util.Collections#enumeration()]
* [javax.xml.bind.annotation.adapters.XMLAdapter]

