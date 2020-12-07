---
title: 「面试」SAP面试凉经
author: bellick
copyright: true
top: 0
date: 2020-11-28 10:18:43
categories:
- 面试
tags:
mathjax:
---

# SAP VT intern 面试凉经

## 1.谈谈对「云」的理解

## 2.谈谈对「微服务」的理解

> 什么是软件架构

软件架构是以组件、组件相互间的关系、组件和环境之间的关系所描述的软件系统的基本组织结构，以及指导其设计和演化的一些原理。

软件架构的原则：

* 关注点分离原则
* 尽量延迟绑定时间（面向服务架构SOA）

分层架构的模式特点：

* 优点：结构简单，易于组织开发。便于独立测试维护
* 缺点：不易实现持续发布和部署，性能代价，可扩展性比较差

面向服务架构SOA：

* 企业总线ESB：为服务间的相互调用提供支持环境,路由服务间的消息，并对消息和数据进行必要的转换 服务编排引擎，可以根据预先定义的脚本对服务消费者与服务提供者之间的交互进行指挥.
* 特点：
  * 优点：服务自身高内聚,服务间松耦合,最小化开发维护中的相互影响良好的互操作性,符合开放标准模组化,高重用性,服务动态识别注册调用
  * 系统复杂性提高,难以测试验证,各独 立服务的演化不可控,中间件容易成为性能瓶颈.

> 微服务

微服务架构关键是**小** 和**自治**，指的是小到一定的粒度并且可以独立的去开发维护部署。

微服务架特点：

* 服务组件化：组件是一个可以独立替换或升级的软件单元，微服务架构实现组件化的方式是**分解成服务**，服务是一种进程外的组件，它通过web服务请求或者RPC通信机制通信。
* 使用服务作为组件的主要原因是：服务可以独立部署；微服务接口定义更加明确
* 围绕业务能力组织：微服务采用产品开发模式，而非项目模式，开发团队负责软件的整个产品周期，持续关注如何帮助用户提高业务能力，实现价值交付
* 内聚和耦合：基于微服务构建的系统目标是尽可能地解耦合和尽可能内聚
* 去中心化：在构建微服务的时候可以根据自己的技术栈选择，服务之间只需约定接口，无需关注内部实现，运维只需要知道服务的部署规范。每个服务管理自己的数据库。服务间抢到无事务协作。基础设施自动化，服务设计高可用。

微服务架构与传统架构的比较：

* 单体：
  * 优点是人人所众知，对IDE友好，便于共享、易于测试、容易部署
  * 缺点是不够灵活，任何细节修改需要把整个应用重新部署，单体较大时，构建时间比较长，不利于频繁部署，阻碍持续交付
* SOA（面向服务架构）：
  * 优点是易维护。消息机制和异步机制的高可靠性、高扩展和高可用性，软件质量提升，可以集成不同的系统，提升开发效率
  * 缺点是过分使用ESB（企业服务总线），需要使用可靠的使得初始投资高，使用形式化方法管理提升了管理的复杂度
* 微服务：
  * 优点
    * 单个服务简单，只关注一个业务功能
    * 团队独立性：每个服务可以由不同的开发团队开发，松耦合
    * 平台无关性：服务可以使用不同的语言/平台开发
    * 通信协议轻量级
  * 运维成本高，分布式系统复杂，同时也让测试变得复杂，消息与并行开发方式使得系统开发门槛增加

微服务的核心模式

* 服务注册与发现：最重要的是心跳机制，如果长时间没有心跳就说明服务提供者和被服务者说明那一方已经失效,断开了连接



## 3.编程题1 - 判断奇偶

```java
// 方法1
public boolean isOdd(int x){
    // 如果模2为0 则为偶数，否则为奇数
    if (x%2==0){
        return false;
    }else{
        return true;
    }
}
// 方法2
public boolean isOdd(int x){
    // 如果模2为1，则为奇数，否则为偶数
    if(x%2==1){
        return true;
    }else{
        return false;
    }
}
// 方法3
public boolean isOdd(int x){
    return x%2 == 1;
}
// 当 n > 0时，两种方法结果一致
// 当 n < 0时，结果就不一样了
// -1 % 2 = -1
// 所以方法2是错误的
// 方法3也是错的
```

模运算的底层算法

```java
A%B = A - (A/B)*B
```

> 在java中，%运算的结果和左操作数具有相同的符号

```java
// 方法4
public boolean isOdd(int x){
    return x%2 != 0;
}
public boolean isOdd(int x){
    x&1 != 0;
}
// 奇数的最后一位总是1
// 位运算，
```



## 4.编程题2 - 类加载过程

```java
class Graph{
    public Graph(){
        System.out.println("before Graph.draw()");
        draw();
        System.out.println("after Graph.draw()");
    }
    public void draw(){
        System.out.println("Graph.draw()");
    }
}

class Circle extends Graph{
    private int radius = 1;
    public Circle(int radius){
        this.radius = radius;
        System.out.println("Circle.radius : " +radius);
    }

    @Override
    public void draw() {
        System.out.println("Circle.draw() radius = " + radius);
    }
}

public class Test {
    public static void main(String[] args) {
        Graph c = new Circle(5);
    }
}

/**
* 输出
* before Graph.draw()
* Circle.draw() radius = 0  ⚠️
* after Graph.draw()
* Circle.radius : 5
*/
```

参考 https://segmentfault.com/a/1190000004527951

> 类初始化过程：
>
> 首先所有类会被编译为class文件。
>
> * 在初始化的时候会先执行Test类中的静态方法，也就是Main方法
> * 在Main方法中 new 一个 Circle对象，传入参数 5
> * 首先会调用Circle的构造函数  Circle(radius=5);
> * 然后Circle的构造函数会隐式调用 this.super() 去调用父类的无参构造函数
> * 在父类的无参构造函数中 
>   1. System.out.println("before Graph.draw()");
>   2. draw(); 
>      * 这一步相当于在 Circle 的构造函数中调用了 this.super() 这个函数中调用了 this.draw();
>      * 由于java的动态绑定，在初始化时并不知道 this.draw() 方法具体是哪个draw，需要到运行时才能绑定
>      * 在运行时，this.draw() 绑定了子类（Circle）的draw方法，但此时 子类的 radius还没有初始化，默认的radius为0
>      * 因此输出 Circle.draw() radius = 0 
>   3. System.out.println("after Graph.draw()");
> * 在子类的构造函数中，初始化 radius 为1，然后执行 this.radius = radius 被设置为5
> * 执行System.out.println("Circle.draw() radius = " + radius); // 此时radius为5

子类对象初始化流程

![CC8F9287B331CE2DF20806076A0E0256](https://tva1.sinaimg.cn/large/0081Kckwly1gl4uwiejm0j30wf0fv7bk.jpg)

## 5.编程题3 - js闭包

```js
var name = "The Window";

var object = {
    name : "My Object",

    getNameFunc : function(){
        return function(){
            return this.name;
        };

    }

};

alert(object.getNameFunc()());
```

```js
var name = "The Window";

var object = {
    name : "My Object",

    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };

    }

};

alert(object.getNameFunc()());
```

在js中，变量的作用域有两种：全局变量和局部变量

* 内部函数可以读取全局变量

  ```js
  var n=999;
  
  function f1(){
      alert(n);
  }
  
  f1(); // 999
  // f1 函数内部可以读取 f1 外部的变量
  ```

* 外部函数不能读取函数内的局部变量

  ```js
  function f1(){
      var n=999;
  }
  
  alert(n); // error
  ```

* 函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明了一个全局变量！

  ```js
  function f1(){
      n=999;// 这个地方没用var，实际上这是在函数内部声明了一个全局变量
  }
  
  f1();
  
  alert(n); // 999 n是全局变量，当然可以读取到
  ```

* 当我们需要在函数外部获取函数内部的变量时，就需要利用函数

  ```js
  function f1(){
  
      var n=999;
  
      function f2(){
          alert(n); // 999
          // 将内部变量封装到函数中，使得外部可以通过调用这个函数来获取此变量，类似于getter？
      }
  
  }
  ```

  链式作用域：子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

  函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1就是不可见的。

* 把内部函数作为返回值，就可以获取内部变量了

  ```js
  function f1(){
  
      var n=999;
  
      function f2(){
          alert(n);
      }
  
      return f2;
  
  }
  
  var result=f1();
  
  result(); // 999
  ```

* 闭包的作用

  * 一个是前面提到的可以读取函数内部的变量
  * 另一个就是让这些变量的值始终保持在内存中

* 一个例子

  ```js
  function f1(){
  
      var n=999;
  
      nAdd=function(){n+=1}
  
      function f2(){
          alert(n);
      }
  
      return f2;
  
  }
  
  var result=f1();
  
  result(); // 999
  
  nAdd();
  
  result(); // 1000
  ```

  > 在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。
  >
  > 为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。
  >
  > 这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

* 闭包的优缺点

  * 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题
    * 解决方法是，在退出函数之前，将不使用的局部变量全部删除
  * 闭包会在父函数外部，改变父函数内部变量的值。
    * 所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

参考 [阮一峰](https://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

评论思考题

```js
function f1(){

    n=999;

    function f2(){
        alert(n);
    }

    return f2;

}

var result=f1();

result(); // 999

可以写成如下的不也一样么？

function f1(){

　　　　n=999;

　　　　return n;

　　}
var result=f1();
alert(result);
```

> 实际上后种方法每次调用 f1 时，都会声明 n = 999，而且 n 无法保留状态值（严格按照你的代码，其实 n 为全局变量，我理解你的本意为 var n = 999;）。
>
> 而第一种 f1 实际上返回的是个匿名函数，这样 n 作用域被另外个 f2 函数作用域所使用，因此它会保留。n 不会被重复声明，且内容会被保存

当内部函数持有外部变量时，内部函数会始终在内存中，这样就可以保留内部变量的状态值。Te



## 6.设计题 - 抢票软件（每个ID30分钟内最多抢5次）

