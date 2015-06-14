---
layout: post
title: <<深入理解C指针>>读书笔记
description: "Understanding and Using C Pointers"
tags: [Notes]
image:
  background: photography.png
---

>指针可以说是C的灵魂，C语言入门很容易，但想精通，就必须要对指针有很透彻的理解。大一上学期学了一遍C语言，然而指针这个最重要的地方，却没有好好的深入研究。这本<<深入理解C指针>>虽然谈不上讲到多少指针的奇巧淫技，但作为复习指针的相关知识，还是可以稍微读读。

## 阅读声明
倒过来读会让指针更容易理解。
￼￼￼
例如：

```
const int *pci;
```

---

| Explanation |  Declaration   |
| ------------- |:-------------:| 
|1.pci is a variable|const int *<strong>pci</strong> | 
|2.pci is a pointer variable| const int <strong> *pci</strong>| 
|3.pci is a pointer variable to an integer|const <strong> int *pci</strong> |
|4.pci is a pointer variable to a constant integer| <strong>const int *pci </strong>|
 
 ---
 
##常量与指针
 
和<<C专家编程>>一样，<<深入理解C指针>>也讲到了常量与指针。

###指向常量的指针

指针定义为指向常量，不能通过指针修改它所引用的值。

```
const int *ptr;
int const *ptr;
```

###指向非常量的常量指针

指针不可变，但所指向的数据可变。指针必须被初始化为指向非常量变量。

```
int *const ptr;
```

###指向常量的常量指针
 
指针本身不能修改，指向的数据也不能通过它来修改。

```
const int *const ptr;
```

###指向『指向常量的常量指针』的指针

```
const int * const * ptr;
```

##动态内存分配

* malloc
* realloc
* calloc

| Function |  Declaration   |
| ------------- |:-------------:| 
|malloc|从堆上分配内存| 
|realloc|在之前分配的内存块基础上，将内存重新分配为更大或者更小的部分|
|calloc|从堆上分配内存并清零|  

###变长数组

使用alloca函数(微软为maalloca)在函数的栈帧上分配内存。函数返回后自动释放内存。

####栈帧
栈帧由以下几种元素组成

* 返回地址
* 局部数据存储
* 参数存储
* 栈指针和基指针

##动态分配二维数组
###动态分配可能不连续的内存

以下代码演示了如何创建一个内存可能不连续的二维数组。

{% highlight c %}
int rows = 2;
int columns = 5;

int **matrix = (int **) malloc(rows * sizeof(int *));

for(int i = 0; i < rows; i++){
	matrix[i] = (int *) malloc(columns * sizeof(int))
}
{% endhighlight %}

###动态分配连续内存

{% highlight c %}
int rows = 2;
int columns = 5;

int **matrix = (int **) malloc(rows * sizeof(int *));
matrix[0] = (int *) malloc(rows * columns * sizeof(int));
for(int i = 0; i < rows; i++){
	matrix[i] = matrix[0] + i * columns;
}
{% endhighlight %}
以上代码演示了第一种技术，第一个malloc分配了一个整数指针数组，一个元素用来存储一行的指针。第二个malloc在存储每行第一个元素的地址。

第二种技术是一次性分配数组所需的所有内存。
{% highlight c %}
int *matrix = (int *) malloc(rows * columns * sizeof(int));
{% endhighlight %}

(To be continued)