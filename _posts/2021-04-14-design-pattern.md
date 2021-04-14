---
layout: post
title: "设计模式整理"
create: 2021-04-14
update: 2021-04-14
categories: blog
tags: [Code]
description: "A Set of Design Pattern"
---

## 单例模式

```java
/**
 * 懒汉式
 */
public class SingletonExample() {
    private volatile SingletonExample instance;

    public SingletonExample getInstance() {
        if (instance == null) {
            synchronized (instance) {
                if (instance == null) {
                    instance = new SingletonExample();
                }
            }
        }

        return instance;
    }
}
```
