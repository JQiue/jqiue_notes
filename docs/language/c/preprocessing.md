---
title: 编译预处理
category: 编程语言
tag: C
author: JQiue
article: false
---

编译预处理是编译程序之前的操作，以#开头，包括：**文件包含，宏定义，条件编译**

他们不是C语言的成分，但C语言离不开他们

## 文件包含

+ 格式：#include<头文件>

## 宏定义

+ 格式：#define 标识符 值

编译之前，预处理程序会把程序中的名字替换成值，是一种完全的文本替换

如果一个宏当中有其他宏的名字，也是会被替换的

如果一个宏的值超过了一行，需要在末尾加`\`

一个宏定义可以没有值

C语言还提供了一些预定义的宏

宏|描述
---|---
`__DATE__`|当前源文件的编泽口期，用 “Mmm dd yyy”形式的字符串常量表示
`__FILE__`|当前源文件的名称，用字符串常量表示
`__LINE__`|当前源义件中的行号，用十进制整数常量表示，它可以随#line指令改变
`__TIME__`|当前源文件的最新编译吋间，用“hh:mm:ss”形式的宁符串常量表示

### 带参数的宏

+ 格式：#define 标识符(参数1,参数2) ((参数1)*(参数2))

可以像函数一样携带参数，但原则是一切都要括号，整个值也要括号，参数出现的每一个地方都要括号，也可以嵌套其他宏

## 条件编译

## 标准库函数

### stdlib.h

+ srand

+ rand

+ exit

+ system

### string.h

+ strcpy
+ strcmp
+ strlen

### math.h

+ ceil
+ floor
+ sqrt
+ pow
+ abs