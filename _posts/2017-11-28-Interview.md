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
- 对于任何非空因
