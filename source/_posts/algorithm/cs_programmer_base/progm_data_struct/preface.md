---
layout: post
title: 基本概念
date: 2015-02-22
category: programmer
tags: [c++,programmer]
toc: true
---

## c++内置类型
内置类型分两组，基本类型和复合类型。基本类型包括整数、浮点数及两者的多种变体。复合类型包括数组、字符串、指针、引用、结构体和共用体。

## c++基本整型
按宽度递增的顺序排序，分别是char, short, int, long其中每种类型都有无符号版本和有符号版本。

## 内存分区
(1)、堆: 由程序员手动分配和释放，完全不同于数据结构中的堆，分配方式类似链表。由**malloc**(c语言)或**new**(c++)来分配，**free**(c语言)和**delete**(c++)释放。若程序员不释放，程序结束时由系统释放。

(2)、栈: 由编译器自动分配和释放的，存放函数的参数值、局部变量的值等。操作方式类似数据结构中的栈。

(3)、全局(静态)存储区: 存放全局变量和静态变量。包括DATA段(全局初始化区)与BSS(Block Started by Symbol)段(全局未初始化区)。其中，初始化的全局变量和静态变量存放在DATA段，未初始化的全局变量和未初始化的静态变量存放在BSS段。程序结束后由由系统释放。

其中BSS段的特点是：在程序执行之前BSS段会自动清0。所以，未初始化的全局变量与静态的变量在程序执行之前已经成为0了。

(4)、文字常量区: 常量字符串就是放在这里的。程序结束后由系统释放。

(5)、程序代码区: 存放函数体的二进制代码。

example
```C
int k = 1;
void main()
{
    int i = 1;
    char *j;
    static int m = 1;
    char *n = “hello”;/*变量n位于栈上，其内容为一地址，指向位于文字常量区的”hello”;此时”hello”在内存中只有一份拷贝;
        而语句char a[]=”hello”;则不同，a是一个位于栈上的有6个元素(含字符串末尾的空字符)的数组，并将”hello”拷贝到它所占
        的内存中，此时”hello”有两份拷贝。*/
    printf(“栈区地址为: 0X%x\n”, j);
    j = (char*)malloc(2);
    free(j);
    printf(“堆区地址为: 0X%x\n”, j);
    printf(“全局变量地址为: 0X%x\n”, &k);
    printf(“静态变量地址为: 0X%x\n”, &m);
    printf(“文字常量区地址为: 0X%x\n”, n);
    printf(“程序区地址为: 0X%x\n”, &main);
}
```
