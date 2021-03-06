---
layout: post
title: js垃圾回收
date: 2014-11-26
category: js
tags: js
toc: true
---
## 垃圾收集
JavaScript具有自动垃圾收集机制，原理很简单，找出那些不再继续使用的变量，然后释放其占用的内存，为此，垃圾收集器收集会会按照固定时间间隔(或代码执行中预订的收集时间),周期性执行这一操作。<!--more-->
垃圾收集器必须跟踪哪个变量有用，哪个变量没用，对于不再有用的变量打上标记，以备将来收回其占用内存。用于标记无用变量可能会因实现而异，但具体到浏览器中实现，则通常有两种策略。
## 标记清除
Javascript中最常用的垃圾收集方式是**标记清除**`mark-and-sweep`。当变量进入环境(例如，在函数中声明一个变量)时，就将这个变量标记为”进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。而当变量离开环境时，则将其标记为“离开环境”。
可以使用任何方式来标记变量。比如，可以通过翻转某个特殊的位来记录一个变量何时进入环境，或者使用一个“进入环境的”变量列表及一个”离开环境”的变量列表来跟踪哪个变量发生了变化。如何标记变量并不重要，关键在于采取什么策略。
垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后在被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成”内存清除”工作。
## 引用计数
另一种不太常见的垃圾收集策略叫做**引用计数**`reference counting`。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。如果同一个值又被赋值给另一个变量，则该值的引用次数减一。当这个值的引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。
但有一个严重的问题**循环引用**。
## 性能问题
垃圾收集器是周期性的运行的，而且如果为变量分配的内存数量很可观，那么回收工作量是相当大的。这种情况下，确定垃圾收集的时间间隔是一个非常重要的问题。
## 管理内存
使用具备垃圾收集机制的语言编写程序，开发人员一般不必关心内存管理的问题。但是Javascript在内存管理及垃圾收集时面临问题与众不同，其中最主要的一个问题，就是分配给Web浏览器的可用内存数量通常要比分配给桌面应用程序的少。这样做的目的主要是出于安全方面的考虑，目的是防止运行Javascript的网页耗尽全部系统内存而导致系统崩溃。内存限制问题不仅会影响给变量分配内存，同时还会影响调用栈以及在一个线程中能够同时执行的语句数量。
因此，确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其设置为null来释放其引用——这个做法叫做**解除引用**`dereferencing`。这一做法适用于大多数全局变量和全局对象的属性。局部变量会在它们离开执行环境时自动被解除引用。
## 小结
Javascript变量可以用来保存两种类型的值：基本类型值和引用类型值。基本类型的值源自一下5种基本数据类型是类型:`Undefined`、`Null`、`Boolean`、`Number`和`String`。基本类型值和引用类型值具有一下特点：
  * 基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中;
  * 从一个变量向另一个变量复制基本类型的值，会创建这个值的一个副本;
  * 引用类型的值是对象，保存在堆内存中;
  * 包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针;
  * 从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终都指向同一个对象;
  * 确定一个值是哪种基本类型可以使用`typeof`操作符，而确定一个值是哪种引用类型可以使用`instanceof`操作符;

所有变量(包括基本类型和引用类型)都存在于一个执行环境(也被称为作用域)当中，这个执行环境决定了变量的生命周期，以及哪一部分代码可以访问其中的变量。以下是关于执行环境的几点总结:
  * 执行环境有全局执行环境(也称为全局环境)和函数执行环境之分;
  * 每次进入一个新执行环境，都会创建一个用于搜索变量和函数的作用域链接;
  * 函数的局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含(父)环境，乃至全局环境;
  * 全局环境只能访问在全局环境中定义的变量和函数，而不能直接访问局部环境中的任何数据;
  * 变量的执行环境有助于确定应该何时释放内存。

Javascript是一门具有自动垃圾收集机制的编程语言，开发人员不必关心内存分配和回收问题。可以对Javascript的垃圾收集例程作如下总结：
  * 离开作用域的值将被自动标记为可以回收，因此将在垃圾收集期间被删除;
  * “标记清除”是目前主流的垃圾收集算法，这种算法的思想是给当前不使用的值加上标记，然后在回收其内存。
  * 另一种垃圾收集算法是”引用计数”，这种算法的思想是跟踪记录所有被引用的次数。Javascript引擎目前都不在使用这种算法。
  * 当代码中存在循环引用现象时，“引用计数”算法就会导致问题;
  * 解除变量的引用不仅有助于消除循环引用现象，而且对垃圾收集也有好处。为了确保有效地回收内存，应该及时解除不再使用的全局对象、全局全局对象属性以及循环引用变量的引用。
