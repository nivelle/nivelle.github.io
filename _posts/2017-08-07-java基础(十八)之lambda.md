---
layout: post
title:  "java基础(十八)之Lambda表达式"
date:   2016-08-07 00:06:05
categories: java
tags: java8
excerpt: java
author: nivelle
---

### 基本语法




```

public class Test {
   public static  void main(String args[]){
       new Thread(() -> System.out.println("Java8 to do it")).start();

       new Thread(new Runnable() {
           @Override
           public void run() {
               System.out.println("Before Java8, too much code for too little to do");
           }
       }).start();
   }
}



```



你可以使用lambda写出如下代码：



```
(params) -> expression

(params) -> statement

(params) -> { statements }

```
