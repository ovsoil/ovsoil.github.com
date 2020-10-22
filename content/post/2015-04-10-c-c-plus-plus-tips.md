---
layout: post
title: "C/C++的一些备注点"
date: 2015-04-10 13:12
comments: true
categories: 
---

作为从应用层面开始接触C/C++的开发者，对这个语言的一些特性和关键字使用很少，在这里记录备忘

<!--more-->

###volatile

volatile


###restrict

restrict是c99标准引入的，它只可以用于限定和约束指针，并表明指针是访问一个数据对象的唯一且初始的方式.即它告诉编译器，所有修改该指针所指向内存中内容的操作都必须通过该指针来修改,而不能通过其它途径(其它变量或指针)来修改;这样做的好处是,能帮助编译器进行更好的优化代码,生成更有效率的汇编代码.如 int *restrict ptr, ptr 指向的内存单元只能被 ptr 访问到，任何同样指向这个内存单元的其他指针都是未定义的，直白点就是无效指针。restrict 的出现是因为 C 语言本身固有的缺陷，C 程序员应当主动地规避这个缺陷，而编译器也会很配合地优化你的代码.
这个关键字据说来源于古老的FORTRAN。有兴趣的看看这个。

考虑下面的例子：

        int ar[10];
        int * restrict restar=(int *)malloc(10*sizeof(int));
        int *par=ar;
        for(n=0;n<10;n++)
        {
            par[n]+=5;
            restar[n]+=5;
            ar[n]*=2;
            par[n]+=3;
            restar[n]+=3;
        }
因为restar是访问分配的内存的唯一且初始的方式，那么编译器可以将上面对restar的操作优化为：`restar[n]+=8;`
而par并不是访问数组ar的唯一方式，`ar[n]*=2`就改变了数组，所以编译器不能进行：

C库中有两个函数可以从一个位置把字节复制到另一个位置。在C99标准下，它们的原型如下：
        
        void * memcpy（void * restrict s1, const void * restrict s2, size_t n);
        void * memove(void * s1, const void * s2, size_t n);

这两个函数均从s2指向的位置复制n字节数据到s1指向的位置，且均返回s1的值。两者之间的差别由关键字restrict造成，即memcpy()可以假定两个内存区域没有重叠。memmove()函数则不做这个假定，因此，复制过程类似于首先将所有字节复制到一个临时缓冲区，然后再复制到最终目的地。如果两个区域存在重叠时使用memcpy()会怎样？其行为是不可预知的，既可以正常工作，也可能失败。在不应该使用memcpy()时，编译器不会禁止使用memcpy()。因此，使用memcpy()时，您必须确保没有重叠区域。这是程序员的任务的一部分。
　　

关键字restrict有两个读者。一个是编译器，它告诉编译器可以自由地做一些有关优化的假定。另一个读者是用户，他告诉用户仅使用满足restrict要求的参数。一般，编译器无法检查您是否遵循了这一限制，如果您蔑视它也就是在让自己冒险。

<!-- create time: 2014-12-12 14:08:32  -->
