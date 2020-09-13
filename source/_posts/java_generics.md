---
title: "Java泛型"
date: 2020-03-05 00:00
tags:
	- Java
	- Java Generics
---



### Java泛型 类型擦除

正确理解泛型概念的首要前提是理解类型擦除(type erasure)。Java中的泛型基本上都是在编译器这个层次来实现的。在生成的Java字节代码中是不包含泛型中的类型信息的。使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个过程就称为类型擦除。如在代码中定义的`List<Object>`和`List<String>`等类型，在编译之后都会变成List。JVM看到的只是`List`，而由泛型附加的类型信息对JVM来说是不可见的。Java编译器会在编译时尽可能的发现可能出错的地方，但是仍然无法避免在运行时刻出现类型转换异常的情况。

### Java泛型特性

- 泛型类并没有自己独有的Class类对象。比如并不存在`List<String>.class`或是`List<Integer>.class`，而只有`List.class`。

- 静态变量是被泛型类的所有实例所共享的。对于声明为`MyClass<T>`的类，访问其中的静态变量的方法仍然是 `MyClass.myStaticVar`。不管是通过`new MyClass<String>`还是`new MyClass<Integer>`创建的对象，都是共享一个静态变量。

- 泛型的类型参数不能用在Java异常处理的`catch`语句中。因为异常处理是由JVM在运行时刻来进行的。由于类型信息被擦除，JVM是无法区分两个异常类型`MyException<String>`和`MyException<Integer>`的。对于JVM来说，它们都是` MyException`类型的。也就无法执行与异常对应的`catch`语句。

  #### 不允许创建参数化类型数组

  ```java
  Pair<String>[] table = new Pair<String>[10];  // Error
  ```

  擦除之后，`table`的类型是`Pair[]`。可以把它转换为`Object[]`:`Object[] objarray = table;`

  数组会记住它的元素类型，如果试图存储其他类型的元素，就会抛出一个`ArrayStoreException`异常：`objarray[0] = "hello"; // Error--component tye is Pair`

  不过对于泛型类型， 擦除会使这种机制无效。

  `objarray[0] = new Pair<Employee>()`能够通过数组存储检查，不过仍会导致一个类型错误。

  处于安全考虑呢，Java不允许创建参数化类型的数组。

### Java泛型与C++模板 差异之处

“泛型编程”这个概念最早就是来源于C++当初设计STL时所引入的模板（Template），而为什么要引入模板呢，因为STL要完成这样一个目标：设计一套通用的，不依赖类型的，高效的的算法（例如`std::sort`）和数据结构（例如`std::list`）。关于通用性，运行时多态（Polymorphism）可以做到（例如很多高级语言的继承（Inheritance）机制，接口（Interface）机制），但是C++作为一门相对底层的语言，对运行效率的要求是很严格的，而运行时多态会影响效率（例如成员函数只有在运行时才知道调用哪个），所以设计STL的人就创造了一种编译时多态技术，即模板。

那什么又是编译时多态呢，简单点说就是让编译器帮我确定类型，我写程序时只要标记下这里我要用“某种类型”的对象，至于具体是什么类型我不关心，你编译器帮我确定，编译完成后在运行时绝对是类型确定的，这样就大大提高了运行效率，反之对编译就增加了很多工作，而且生成的目标代码也会大大增加。所以对C++来说，所谓“泛型（Generics）”，并不是说编译器不知道类型，而是针对程序员来说的，这也正是通用性的体现。

在 C++ 模板中，编译器使用提供的类型参数来扩充模板，因此，为 `List<A>` 生成的 C++ 代码不同于为 `List<B>` 生成的代码，`List<A>` 和 `List<B>` 实际上是两个不同的类。而 Java 中的泛型则以不同的方式实现，编译器仅仅对这些类型参数进行擦除和替换。类型 `ArrayList<Integer>` 和 `ArrayList<String>` 的对象共享相同的类，并且只存在一个 `ArrayList` 类


## References:

1. Java核心技术
2. [Java泛型：类型檫除、模板和泛型传递](https://codefine.site/1759.html)
