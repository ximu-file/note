# Java8内置函数式接口
#技术积累/Java

Java8里面新增的一个非常重要的特性就是函数式接口，functional Interface。 函数式接口往往与lambda表达式一起使用。

下面介绍一下Java8中新增的常用的函数式接口：

# 1. Predicates
先给出这个接口在JDK8中的简化定义：
```java
@FunctionalInterface
public interface Predicate<T> {
	boolean test(T t);
}
```
Predicate是一个布尔类型的函数，该函数只有一个输入参数。Predicate接口包含了多种默认方法，用于处理复杂的逻辑动词（and, or，negate）。

对于这个接口的实例化一般都是通过lambda表达式实现的，因为接口只有一个没有实现的方法，所以我们完全可以使用lambda表达式简化实例化，而不用使用匿名内部类，如下例子：

```java
Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");              // true
predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
```

# Functions
```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```
Function接口接收一个参数，并返回单一的结果。默认方法可以将多个函数串在一起（compse, andThen）。

如下例子：
```java
// 输入是 String 输出是 Integer
Function<String, Integer> toInteger = Integer::valueOf;
// 输入是 String 输出也是 String
Function<String, String> backToString = toInteger.andThen(String::valueOf);

String result = backToString.apply("123");     // "123"
System.out.println(result);
```

# Suppliers
```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
Supplier接口产生一个给定类型的结果。与Function不同的是，Supplier没有输入参数。

下面给出一个实例：
```java
Supplier<Person> personSupplier = Person::new;
personSupplier.get();   // 就相当于new Person
```

# Consumers
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```
Consumer代表了在一个输入参数上需要进行的操作，而且没有返回值。 比如如下例子：
```java
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);
greeter.accept(new Person("Luke", "Skywalker"));
```

# Comparators
```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```
Comparator接口在早期的Java版本中非常著名。Java 8 为这个接口添加了不同的默认方法。

同样给出一个示例：
```java
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);

Person p1 = new Person("John", "Doe");
Person p2 = new Person("Alice", "Wonderland");

comparator.compare(p1, p2);             // > 0
comparator.reversed().compare(p1, p2);  // < 0
```



















