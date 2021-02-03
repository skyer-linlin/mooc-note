# 设计模式

## 07 理论四：哪些代码看似是面向对象的，实际是面向过程的

### 1. 滥用 getter、setter 方法

在设计实现类的时候，除非真的需要，否则尽量不要给属性定义 setter 方法。除此之外，尽管 getter 方法相对 setter 方法要安全些，但是如果返回的是集合容器，那也要防范集合内部数据被修改的风险。

### 2.Constants 类、Utils 类的设计问题

对于这两种类的设计，我们尽量能做到职责单一，定义一些细化的小类，比如 RedisConstants、FileUtils，而不是定义一个大而全的 Constants 类、Utils 类。除此之外，如果能将这些类中的属性和方法，划分归并到其他业务类中，那是最好不过的了，能极大地提高类的内聚性和代码的可复用性。

### 3. 基于贫血模型的开发模式

关于这一部分，我们只讲了为什么这种开发模式是彻彻底底的面向过程编程风格的。这是因为数据和操作是分开定义在 VO/BO/Entity 和 Controler/Service/Repository 中的。今天，你只需要掌握这一点就可以了。为什么这种开发模式如此流行？如何规避面向过程编程的弊端？有没有更好的可替代的开发模式？相关的更多问题，我们在面向对象实战篇中会一一讲解。

## 08 理论五：接口和抽象类比较

抽象类特性：

- 抽象类不允许被实例化，只能被继承
- 抽象类可以包含属性和方法
- 子类继承抽象类，必须实现抽象类中的所有抽象方法

接口特性：

- 接口不能包含属性
- 接口只能声明方法，不能包含代码实现
- 类实现接口的时候，必须实现接口中声明的方法

从设计角度的比较：

- 抽象类：一中特殊的类，和实现类是 is-a 关系
- 接口：形象的说法是协议，表示具有某些功能，是一中 has-a 关系

使用场景：

- 表示 is-a 关系，为了实现代码复用，使用抽象类
- 表示 has-a 关系，为了解决抽象而非代码复用，使用接口

## 09 为什么基于接口而非实现编程

接口的定义只表明做什么，而不是怎么做

基于接口而非实现编程原则：

- 行数的命名不应该暴露任何实现细节
- 封装具体的实现细节
- 为实现类定义抽象的接口

## 10 组合还是继承

- 继承(Inheritance)：利用 extends 来扩展一个基类。is-a 的关系。
- 组合(composition)：一个类的定义中使用其他对象。has-a 的关系。
- 委托(delegation)：一个对象请求另一个对象的功能，捕获一个操作并将其发送到另一个对象。有 uses-a, owns-a, has-a 三种关系。

继承最大的问题就在于：继承层次过深、继承关系过于复杂会影响到代码的可读性和可维护性。

利用组合（composition）、接口、委托（delegation）三个技术手段，一块儿来解决继承存在的问题.

```java
// 通过组合和委托技术来消除代码重复
public interface Flyable {
  void fly()；
}
public class FlyAbility implements Flyable {
  @Override
  public void fly() { //... }
}
//省略Tweetable/TweetAbility/EggLayable/EggLayAbility

public class Ostrich implements Tweetable, EggLayable {//鸵鸟
  private TweetAbility tweetAbility = new TweetAbility(); //组合
  private EggLayAbility eggLayAbility = new EggLayAbility(); //组合
  //... 省略其他属性和方法...
  @Override
  public void tweet() {
    tweetAbility.tweet(); // 委托
  }
  @Override
  public void layEgg() {
    eggLayAbility.layEgg(); // 委托
  }
}
```

继承主要有三个作用：表示 is-a 关系，支持多态特性，代码复用。而这三个作用都可以通过其他技术手段来达成。比如 is-a 关系，我们可以通过组合和接口的 has-a 关系来替代；多态特性我们可以利用接口来实现；代码复用我们可以通过组合和委托来实现。所以，从理论上讲，通过组合、接口、委托三个技术手段，我们完全可以替换掉继承，在项目中不用或者少用继承关系，特别是一些复杂的继承关系

如果类之间的继承结构稳定（不会轻易改变），继承层次比较浅（比如，最多有两层继承关系），继承关系不复杂，我们就可以大胆地使用继承。反之，系统越不稳定，继承层次很深，继承关系复杂，我们就尽量使用组合来替代继承。

还有一些特殊的场景要求我们必须使用继承。如果你不能改变一个函数的入参类型，而入参又非接口，为了支持多态，只能采用继承来实现。比如下面这样一段代码，其中 FeignClient 是一个外部类，我们没有权限去修改这部分代码，但是我们希望能重写这个类在运行时执行的 encode() 函数。这个时候，我们只能采用继承来实现了。

```java

public class FeignClient { // Feign Client框架代码
  //...省略其他代码...
  public void encode(String url) { //... }
}

public void demofunction(FeignClient feignClient) {
  //...
  feignClient.encode(url);
  //...
}

public class CustomizedFeignClient extends FeignClient {
  @Override
  public void encode(String url) { //...重写encode的实现...}
}

// 调用
FeignClient client = new CustomizedFeignClient();
demofunction(client);
```

## 17 里式替换原则

子类对象（object of subtype/derived class）能够替换程序（program）中父类对象（object of base/parent class）出现的任何地方，并且保证原来程序的逻辑行为（behavior）不变及正确性不被破坏。

## 20 控制反转、依赖反转、依赖注入

依赖注入：不通过 new（）方式来在类内部创建依赖类对象，而是将依赖类在外部创建好之后，通过构造函数、函数参数等方式传递进来。
