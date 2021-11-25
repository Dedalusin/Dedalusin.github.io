---
title: effective java阅读1
date: 2021-11-25
tags: java
category: 阅读
---

### 创建和销毁对象

1. 避免创建不必要的对象。

   比如使用正则表达式matche字符串时，可以将其显式编译为一个pattern实例（不可变），当需要比较时，直接重用该实例。

2. 避免使用终结方法和清除方法。

   - 永远也不要依赖终结方法或者清除方法来更新重要的持久状态。因为根本就不保证会被执行。
   - 不要被Sysme.gc和System.runFinalization这两个方法迷惑。理由同上。
   - 使用终结方法和清除方法有一个非常严重的性能损失。因为它会阻止有效的垃圾回收。
   - 终结方法有一个严重的安全问题。（终结方法攻击）
   - 如果确实有资源需要终止，可以使其实现AutoCloseable。一般的终结方法只是充当close的安全网。

3. try-with-resources优先于try-finally。（在有嵌套try结构时有效避免问题，资源需要实现AutoCloseable接口）