---
title: effective java阅读2
date: 2021-11-25
tags: java
category: 阅读
---

### 第三章 对于所有对象都通用的方法

1. 关于equals

   - 覆盖equals时必须要覆盖hashcode：map中先是由hashcode散列确定位置再由equals比较，假如两个一致却没覆盖hashcode便会导致map中存有重复的目标对象。

   - 不要将equals声明中的object对象替换为其他类型

     ```java  
     // bad example，这并没有覆盖方法，而是重载
     public boolean equals(MyClass o){}
     
     // good 以Integer的equals方法为例
     public boolean equals(Object obj) {
             if (obj instanceof Integer) {
                 return value == ((Integer)obj).intValue();
             }
             return false;
         }
     ```

     > 有时为了避免这种覆盖写成重载的情况出现，可以使用@Override进行注解

2. 始终要覆盖toString() ： 提供好的toString实现可以使类使用起来更加舒适便于调试。
   - 无论是否制定格式，都应该在文档中明确表明意图。
   - 无论是否制定格式，都为toString返回值中包含的所有信息提供一种可以通过编程访问的途径。

3. 谨慎覆盖clone()
   - 不可变的类永远都不应该提供clone方法。
   - 实际上，clone方法就是另一个构造器；必须确保它不会伤害到原有对象，并确保正确地创建创建被克隆对象中的约束条件。
   - 一般clone方法采用先super.clone，再逐域对象迭代、递归复制。
   - 对象拷贝的更好方法是提供一个拷贝构造器。（数组最好利用clone复制）

4. 考虑实现Comparable接口

   - 一旦实现了comparable接口，它就可以跟许多泛型算法以及依赖于该接口的集合实现进行协作。

   - 当比较依赖于两个值之间的区别时，不要使用以下代码。

     ```java  
     // bad example 会造成整数溢出
     static Comparator<Object> hashCodeOrder = new Comparator<>(){
         public int compare(Object o1, Object o2) {
             return o1.hashCode() - o2.hashCode();
         }
     }
     // good example
     static Comparator<Object> hashCodeOrder = new Comparator<>(){
         public int compare(Object o1, Object o2) {
             return Integer.compare(o1.hashCode(), o2.hashCode());
         }
     }
     // 使用比较器构造方法
     static Comparator<Object> hashCodeOrder = Comparator.comparingInt(o -> o.hashCode());
     ```

     

