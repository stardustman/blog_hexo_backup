---
title: asm-stack-management
date: 2019-05-28 19:32:16
tags: ["asm"]
---
# instruction suffiex
缩写 | 全称 | 位数
---- | ---- | ----
b | byte | 8bit
w | word| 16bit
l | long| 32bit
q | quad| 64bit

# stack management
## pushl %eax
等价于
```
subl $4, %esp //分配4个字节的空间,所谓的栈向下生长
movl %eax, (%esp) //将 eax 的值复制到 esp 指到的内存地址
```
> put value of eax onto stack
## popl %eax
等价于
```
movl (%esp),%eax //将 esp 的值复制到 eax
addl $4,%esp //回收空间
```
# 函数栈帧
```c
void swap(int a,int b){
    int tmp = a;
    a = b;
    b = tmp;
}
```
```
// 64 bit 机器 , AT&T 风格的汇编
swap(int, int):
        pushq   %rbp // 上一个栈帧的基地址压栈 等价于 subl $8, %rsp; movq %rbp,(%rsp)
        movq    %rsp, %rbp // 开辟新的函数栈帧,也就是形成一个新的栈的基地址
        movl    %edi, -20(%rbp) //参数 a
        movl    %esi, -24(%rbp) //参数 b
        movl    -20(%rbp), %eax // 把 a 赋值给 %eax
        movl    %eax, -4(%rbp)  // 把 %eax（a）赋值给 %rbp - 4（a） 的地址处
        movl    -24(%rbp), %eax // 把 b 赋值给 % eax（b）
        movl    %eax, -20(%rbp) // 把 %eax （b）赋值给 %rbp - 20（b） 的地址处,完成 b 的交换
        movl    -4(%rbp), %eax  // 把 %rbp - 4（a） 赋值给 %eax （a）
        movl    %eax, -24(%rbp) // 把 %eax (a) 赋值给 %rbp - 24 的地址处,完成 a 的交换
        nop // 延时
        popq    %rbp // 等价于 movq (%rsp),%rbp ; 上一个函数栈帧的基地址恢复;addq $8,%rsp; 上一个函数的%rsp恢复
        ret // popq %rip 函数调用会把 return address 压入栈帧,Call instruction pushes return address (old EIP) onto stack
```

