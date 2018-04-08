---
title: 关于变量 Objects...objects 和Object[] objects的区别
date: 2018-03-16 23:08:45
categories:
- JAVA
- 后台

tags: 
- Objects
- java
---



# 关于变量 Objects...objects 和Object[] objects的区别

上一篇用到Objects...objects 和Object[] objects的遇到点小问题，于是我去做了个实验，关于这两个变量传参的问题
代码如下

```
package com.yck.test;

public class ObjectsTest
{
    public static String  function(Object...objects)
    {
        return "success";
    }
    public static String func(Object[] objs)
    {
        return "victory";
        
    }
    public static void main(String[] args)
    {
        System.out.println("function(Objects...object) 不带参数"+function()); 
        //System.out.println("func(Object[] objs) 不带参数"+func()); //自动报错
        
        System.out.println("function(Objects...object) 带单个参数"+function(1)); 
        //System.out.println("func(Object[] objs) 带单个参数"+func(1)); //自动报错
        //System.out.println("func(Object[] objs) 带单个参数"+func(1)); //自动报错
        
        Object[] objs = {1,"String",true};
        System.out.println("function(Objects...object) 带数组参数"+function(objs)); 
        System.out.println("func(Object[] objs) 带数组参数"+func(objs));
        
        System.out.println("function(Objects...object) 带多个变量"+function(1,"hello",true));     
    }

}
```

 
结果如下
![image](http://p4pm1p21u.bkt.clouddn.com/object.png)

很明显，我们可以得出以下结论

当形参为Object[]数组时，调用该方法必须为一个数组
当形参为Object...objects时，调用就相当灵活了，可以不带参数，可以带一个参数或者多个参数，也可以带数组作为参数
