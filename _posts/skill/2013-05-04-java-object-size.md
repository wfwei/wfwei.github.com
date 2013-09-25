---
layout: post
title: "Java对象大小"
categories: [skill, java]
---

##Java Object Heap Size

这里只考虑*普通对象*在*HotSpot虚拟机*上的堆内存占用情况

* 一般计算公式:` 对象头 + primitive变量 + 引用变量 + Padding `   
这里的变量都是成员变量，类变量或常量都会放在运行时常量池（方法区的一部分）

    * 对象头：包括对象的类型信息，ID以及一些状态位，一般是8字节(数组要8+4=12字节，4字节是存放数组长度length)

    * primitive类型:

        <table border="">
          <tbody><tr><th>Java type</th><th>Bytes required</th></tr>
          <tr><td><tt>boolean</tt></td><td rowspan="2">1</td></tr>
          <tr><td><tt>byte</tt></td></tr>
          <tr><td><tt>char</tt></td><td rowspan="2">2</td></tr>
          <tr><td><tt>short</tt></td></tr>
          <tr><td><tt>int</tt></td><td rowspan="2">4</td></tr>
          <tr><td><tt>float</tt></td></tr>
          <tr><td><tt>long</tt></td><td rowspan="2">8</td></tr>
          <tr><td><tt>double</tt></td></tr>
        </tbody></table>

    * 引用变量：4字节
    * Padding填补：一个对象大小需要是8的整倍数

* 一般例子：
    * 原始的Object对象：8字节（8+0+0+0）
    * 只含有一个boolean变量的类:16字节（8+1+0+7）//如果要优化，可以使用BitSet类
    * 只含有8个boolean变量的类:16字节（8+8+0+0）
    * 只含有2个long，3个引用，1个boolean变量的类:40字节(8+(2×8+1)+3×4+3)

* 数组
    * 一维数组，除了对象头12(8+4)，没有什么特别
    * 二维数组，数组中嵌套数组，分为外围数组和内部数组，都是数组对象(和c中直接划分一块区域用指针计算行列不同)
    * 1×10 boolean数组，24字节 (12+1×10+0+2) //每个boolean仍用1个字节，避免浪费可以使用BitSet
    * 10×10 int数组，616字节，包括一个外层数组对象56字节(12+0+10*4+4)和10个内部数组对象56字节(12+10×4+0+4)
    * 10×10 int数组在C里面只有10×10×4=400字节的内存占用

* [Ref](http://www.javamex.com/tutorials/memory/object_memory_usage.shtml)
