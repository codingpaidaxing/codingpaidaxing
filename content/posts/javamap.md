---
title: "Java-Map"
date: 2023-01-13T23:05:22+08:00
--- 


Java Map解锁新姿势，相信绝大数人并不知晓；相信我，你懂的。

## 简单Map的操作

说到Java Map的操作，其实有很多种，但是归结起来无非就两种，其中一种便是简单Map的操作，

### 简单Map

简单Map指的是：value是个atomic value，比如：

```Java
Map<K, Integer>;
Map<K, Character>;
```

上面的K是个generic type(也就是泛型)。

### 复杂的Map

所谓的复杂的Map指的就是，value不是一个atomic value，常见的一个情况就是value是一个collection，比如：

```java
Map<K, Collection<V>>
Map<K, List<V>>
Map<K, Set<V>>
```

其实简单的Map操作起来还是比较方便的，这里不再一一举例。
但是通常情况下我们使用的Map更多情况下是一个复杂Map，如果像下面这样编写的一个代码，它就有可能抛出一个`NullPointerException`

```java
Map<K, List<V>> map = new HashMap<>();
map.get(key).add(val);
```

上述代码之所以会出现空指针的异常，主要原因就是当map并不包含key时。（`map.get(key).add(val)`）也就等价于`null.add(val)`
在我们日常工作开发中通常采用的解决方案，代码如下：

```java
Map<K, List<V>> map = new HashMap<>();
if (!map.containsKey(key)) {
    map.put(key, new ArrayList<>());
}
map.get(key).add(val);
```

上述方案便可规避掉`NullPointerException`异常，但是代码看上去过于冗长。

### 重点来了！解锁复杂Map避免空指针异常的简化代码

```java
Map<K, List<V>> map = new HashMap<>();
map.computerIfAbsent(key, k-> new ArrayList<>());
map.get(key).add(val);
```

代码是不是看上去简化清爽了许多。
关于`computeIfAbsent`的Java文档可以参考：[computeIfAbsent](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html#computeIfAbsent-K-java.util.function.Function)

当然如此清爽代码的解决方案不止一种，比如还有`Guava Multimap`

### 使用Guava Multimap解锁新姿势(该方式代码更加简洁)

```java
Multimap<K, V> multimap = ArrayListMultimap.create();
multimap.put(key,val);
```

该方法唯一不足的一点就是`Guava`并不是Java标准库的一部分，所以如果想使用Guava Multimap来解决的话需要通过Maven dependency在你的项目中引入Guava；如下方式

```java
<!--
  https://mvnrepository.com/artifact/com.google.guava/guava --
  >
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>30.1.1-jre</version>
</dependency>
```

关于Multimap的Java文档在此处：[Multimap](https://guava.dev/releases/23.0/api/docs/com/google/common/collect/Multimap.html)