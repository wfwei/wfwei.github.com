---
layout: post
title: 设计模式
Author: wfwei - cf.wfwei@gmail.com
Last modified: 2013-09-18 14:56:07
categories:[skill]
---

###单例模式(Singleton)

要求单例类最多只能有一个实例对象

最简单的实现是，在单例类内保存静态实例对象，并将构造方法生命为`private`的，防止调用者构造新对象

    class Singleton{
        private static Singleton instance = new Singleton();
        private Singleton(){}
        public static Singleton getInstance(){
            return instance;
        }
    }

这种实现已经足够满足‘单例’的要求了，但存在一个问题，当第一次加载Singleton类的时候，其实例对象就生成了，并且一直存活到类被回收(类的Class对象是放在JVM的方法区的，方法区也称`Permonent Area`，回收频率是很低的)，这当单例类比较大或内存吃紧的时候，代价是比较高昂的！

一个简单的改进是延迟对象的生成，比如当第一次用到实例对象的时候才去构造对象

    class Singleton{
        private static Singleton instance;
        private Singleton(){}
        public static Singleton getInstance(){
            if(instance==null)
                instance = new Singleton();
            return instance;
        }
    }

这样虽然可以延迟对象的生成时间，但却引发了一个问题：线程安全！

考虑多个线程同时访问`getInstance()`方法时，就可能会生成多个对象，这就不再‘单例’了

如果程序不存在多线程问题，那么这个方案已经非常好了，但并发编程是非常普遍的，还需要改进这个设计

为了实现线程安全，最简单的方法就是在方法上加上`synchronized`的关键字

    class Singleton{
        private static Singleton instance;
        private Singleton(){}
        public static synchronized Singleton getInstance(){
            if(instance==null)
                instance = new Singleton();
            return instance;
        }
    }

这种方式有效，但有点‘粗暴’：如果该实例访问比较频繁的话，`synchronized`整个方法会带来巨大的性能损失

继续改进，其实并不是每次访问都需要`synchronized`：如果`instance`早就实例化了，那么直接返回不就好了么！

    class Singleton{
        private static volatile Singleton instance;
        private Singleton(){}
        public static Singleton getInstance(){
            if(instance==null){ // 只有instance还没有实例化的时候才需要进入synchronized区域
                synchronized(Singleton.class){
                    if(instance==null)
                        instance = new Singleton();
                }
            }
            return instance;
        }
    }

如代码中的注释部分，只有当`instance`为空的时候才`synchronized`对象创建过程

注意，这里`instance`前加了一个修饰词`volative`，`volatile`保证了`instance`的可见性，具体参考[这里](/posts/jvm-note3/)

上面这种方式已经很好了，但还是存在并发处理，有一种结合工厂方法的做法很取巧：

    class SingletonFactory {
        public static class Singleton{
            public static Singleton instance = new Singleton();
        }     
        public static Singleton getInstance(){
            return Singleton.instance;    
        }
    }

参考： [单例模式的几种实现](http://www.cnblogs.com/ykt/archive/2011/11/24/2261251.html)


### TODO 工厂模式
