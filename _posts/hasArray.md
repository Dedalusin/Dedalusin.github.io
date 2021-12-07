---
title: 为什么要有hasArray()？
date: 2021-12-07
tags: java
category: 问题
---

直接贴StackOverflow  

<https://stackoverflow.com/questions/55442531/why-do-we-ever-need-hasarray-method-in-charbuffer>

----

正文：

The condition for `hasArray` is  
    
```java   
(hb != null) && !isReadOnly
```

------
  
`isReadOnly` changes if you use `asReadOnlyBuffer`
  
```java   
CharBuffer.allocate(20).asReadOnlyBuffer();
```

for example.
So yes, we need it.

------

Take this line

```java  
final CharBuffer cb = instance.getCharBuffer(...);
```

Is it read-only or not? Does it hold a valid `char[]` array? We don't really know. If we do

```java  
cb.array();
```

and it is a read-only Buffer, we get a `ReadOnlyBufferException`.
If it isn't backed by a `char[]` array we get a `UnsupportedOperationException`.

So what we *might* do is

```java  
if (cb.hasArray()) {
   final char[] arr = cb.array();
}
```

Now we are `Exception`-safe.
Also, you can be sure Oracle/OpenJDK/whateverJDK engineers know what they're doing ;)

