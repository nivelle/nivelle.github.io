---
layout: post
title:  "Interview summary"
date:   2017-11-28 01:06:05
categories: 技术
tags: Interview
excerpt: 常见数据结构
---

### 数据结构

#### hashMap

##### HashMap中的两个方法

hashCode和equals

```

//jni,调用底层其他语言实现
public native int hashCode();

// 默认同==,直接比较对象
public boolean equals(object obj){
    return (this == obj);
}

```

String 类中重写了equals方法,比较字符串值,看下源码:

```

public boolean equals(Object anObject){
    if(this == anObject){
        return true;
    }
    if(anObject instanceof String){
        String anotherString =(String)anObject;
        int n = value.length; //value String 类内部标识String组成的字符数组
        if(n == anotherString.value.length){
            char v1[] = value;
            char v2[] = anotherString.value;
            int i =0;
            while(n-- !=0){
                if(v1[i]!=v2[i])
                {
                    return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
}


```

重写equals要满足几个条件:

- 自反性:对于任何非空引用值 x，x.equals(x) 都应返回 true
- 对称性:对于任何非空引用值 x 和 y，当且仅当 y.equals(x) 返回 true 时，x.equals(y) 才应返回 true。 
- 传递性:对于任何非空引用值 x、y 和 z，如果 x.equals(y) 返回 true，并且 y.equals(z) 返回 true，那么 x.equals(z) 应返回 true。
- 一致性:对于任何非空引用值 x 和 y，多次调用 x.equals(y) 始终返回 true 或始终返回 false，前提是对象上 equals 比较中所用的信息没有被修改。
- 对于任何非空引用x,x.equals(null)都应该返回false

Object 类的equals方法实现对象上差别可能性最大的相等关系;对于任何非空引用值x和y,当且仅当x和y引用同一个对象时,此方法才返回true(x==y具有值true).当次方法被重写时,通常有必要重写hashCode方法,以维护hashCode方法的常规协定,该协定申明相等对象必须有相等的哈希码.


下面来说说hashCode方法,这个方法我们平时通常是用不到的,它是为哈希家族的集合类框架(hashMap,HashSet,HashTable)提供服务,hashCode的常规协定是:

- 在java应用程序执行期间,在同一对象上多次调用hashCode方法时,必须一致地返回相同的整数,前提是对象上equals比较中用的信息没有被修改.从某一应用程序的一次执行到同一应用程序的另一次执行,该整数无需保持一致.
- 如果根据equals(object)方法,两个对象时相等的,那么在两个对象中的每个对象上调用hashCode方法都必须生成相同的整数结果.
- 以下情况不是必需的:如果根据equals(Object)方法,两个对象不相等,那么在两个对象中任一对象上调用hashCode方法必定会生成不同的整数结果.但是,为不相等的对象生成不同整数结果可以提高哈希表的性能.


### java.util.HashMap<K,V>

java.lang.object 
    继承者
      java.util.AbstractMap<K,V>
         继承者
           java.util.HashMap<K,V>
          

所有已经实现的接口:

```
Serializable,Cloneable,Map<K,V>

```
直接已知子类:


```
LinkedHashMap,PrinterStateReasons

```

HashMap中我们最常用的就是put(K,V)和get(K).HashMap的K值是唯一的,如何保证唯一性呢?如果用equals()比较,随着内部元素的增多,put和get的效率会越来越低,这里的时间复杂度是O(n). 实际上,HashMap很少会用到equals方法,因为其内通过一个哈希表管理所有元素,哈希是通过hash单词音译过来的,也可以称为散列表,哈希算法可以快速的存取元素,当我们调用put存值时,HashMap首先会调用k的hashCode方法,获取哈希码,通过哈希码快速找到某个存放位置,这个位置可以被称为bucketIndex,通过上面所述hashCode的协定可知,如果hashCode不同,equals一定为false,如果hashCode相同,equals不一定为true.所以,hashCode可能存在冲突的情况,专业叫碰撞,当碰撞发生时,计算出的bucketIndex也是相同的,这时会取到bucketIndex位置已存储的元素,最终通过equals来比较,equals方法就是哈希冲突的时候才会执行的方法.HashMap通过hashCode和equals最终判断出K是否已存在，如果已存在，则使用新V值替换旧V值，并返回旧V值，如果不存在 ，则存放新的键值对<K, V>到bucketIndex位置。

![image](http://7xpuj1.com1.z0.glb.clouddn.com/hashMap.jpg)


由以上图可知:

- HashMap通过键的hashCode来快速的存取元素。
- 当不同的对象hashCode发生碰撞时，HashMap通过单链表来解决，将新元素加入链表表头，通过next指向原有的元素。单链表在Java中的实现就是对象的引用(复合)。
