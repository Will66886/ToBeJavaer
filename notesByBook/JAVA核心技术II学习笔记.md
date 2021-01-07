# Core Java Volume II Study Notes

## 第一章、Java8的流库

### 1.1、从迭代到流的操作

流与集合的区别：

1. 流并不存储其元素。这些元素可能存储在底层的集合中，或者是按需生成的。
2. 流的操作不会修改器数据源。
3. 流的操作是尽可能惰性执行的。这意味着直至需要其结果时，操作才会执行。

java.util.stream.Stream\<T>==8==

- Stream\<T> filter(Predicate<? super T> p)：

  产生一个流，其中包含当前流中满p的所有元素

- long count()：

  产生当前流中元素的数量。这是一个终止操作

java.util.Collection\<E>

- default Stream\<E> stream()==8==

- default Stream\<E> parallelstream()==8==：

  产生当前集合中所有元素的顺序流或并行流。

### 1.2、流的创建

可以用Collection接口的Stream方法将任何集合转换为一个流。

```java
List<String> list = List.of(new String[]{"word1", "word2"});
Stream<String> words =list.stream();
```

还可以使用静态方法Stream.of将数组转换为流

```java
Stream<String> words = Stream.of(new String[]{"word1", "word2"});
```

of方法具有可变长参数，因此我们可以构建具有任意数量引元的流：、

```java
Stream<String> words = Stream.of("gently","down","the","stream");
```

使用Array.stream(array,from,to)可以用数组中的一部分元素来创建一个流

Stream.empty可以创建无参数的流

Stream接口有两个用于创建无限流的静态方法：

1. generate方法会接受一个不包含任何引元的函数(或者技术上讲，是一个Supplier\<T>接口的对象)。无论何时，只需要一个流类型的值，该函数就会被调用以产生一个这样的值

   ```java
   Stream<String> echos = Stream.generate(() -> "Echo");
   Stream<Double> randoms = Stream.generate(Math::random);
   ```

2. 如果要产生像0 1 2 3 ...这样的序列，可以使用iterate方法。它会接受一个“种子”值，以及一个函数(从技术上讲，是一个UnaryOperator\<T>)，并且会反复地讲该函数应用到之前的结果上

   ```java
   Stream<BigInteger> integers = Stream.iterate(BigInteger.ZERO,n -> n.add(BigInteger.ONE));
   ```

   如果希望获得有限序列，则需要添加一个为此来描述迭代应该如何结束

   ```java
   BigInteger limit = new BigInteger("100000000");
   Stream<BigInteger> integers2 
       = Stream.iterate(BigInteger.ZERO
                        ,n -> n.compareTo(limit) < 0
                        ,n -> n.add(BigInteger.ONE));
   ```

最后，Stream.ofNullable方法会用一个对象来创建一个非常短的流。如果该对象为null，那么这个流的长度就为0；否则为1，即只包含该对象。这个方法与flatMap相结合时最有用

Java API中还有很多方法可以产生流。

如果我们持有的Iterable对象不是集合，那么可以通过下面的调用将其转换为一个流：

```java
StreamSupport.stream(iterable.spliterator(),false);
```

如果我们持有的是Iterator对象，并且希望得到一个由它的结果构成的流，可以使用以下方法

```java
StreamSupport.stream(Spliterators.spliteratorUnknownSize(iterator,Spliterator.ORDERED),false);
```

最重要的是，在执行流的操作时，我们并没有修改流背后的集合。记住，流并没有收集其数据，数据一直存储在单独的集合中。如果修改了该集合，那么流操作的结果就变成未定义的。JDK文档称这种要求为不干涉性

java.util.stream.Stream\<T>==8==

- static \<T> Stream\<T> of(T... values) ：

  产生一个元素为给定值的流

- static \<T> Stream\<T> empty()：

  产生一个不包含任何元素的流

- static \<T> Stream\<T> generate(Supplier<? extends T> s)：

  产生一个无限流，它的值是通过反复调用函数s而构建的。

- static \<T> Stream\<T> iterate(T seed, UnaryOperator\<T> f) 

- static \<T> Stream\<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator\<T> f)：

  产生一个无限流，它的元素包含seed、在seed上调用f产生的值、在前一个元素上调用f产生的值，等等。第一个方法会产生一个无限流，而第二个方法的流会在碰到第一个不满足hashNext谓词的元素时终止。

- static \<T> Stream\<T> ofNullable(T t) ==9==：

  如果t为null，返回一个空流，否则返回包含t的流

java.util.Spliterators ==8==

- static \<T> Spliterator\<T> spliteratorUnknownSize(Iterator<? extends T> iterator, int characteristics)：

  用给定的特性(一种包含诸如Spliterator.ORDERD之类的常量的位模式)将一个迭代器转换为一个具有未知尺寸的可分割的迭代器。

java.util.Arrays

- static \<T> Stream\<T> stream(T[] array, int startInclusive, int endExclusive) ==8==：

  产生一个流，它的元素是由数组中指定范围内的元素构成的

java.util.regex.Pattern 

- Stream\<String> splitAsStream(CharSequence input) ==8==：

  产生一个流，它的元素是输入中由该模式界定的部分

java.nio.file.Files ==7==

- static Stream\<String> lines(Path path) ==8==

- static Stream\<String> lines(Path path, Charset cs) ==8==：

  产生一个流，它的元素是指定文件中的行，该文件的字符集为UTF-8，或者为指定的字符集

java.util.stream.StreamSupport ==8==

- static \<T> Stream\<T> stream(Spliterator\<T> spliterator, boolean parallel) 

  产生一个流，它包含了有给定的可分割迭代器产生的值

java.lang.Iterable\<T>

- default Spliterator\<T> spliterator() ==8==：

  为这个Iterable产生一个可分割的迭代器。默认实现不分割也不报告尺寸

java.util.Scanner 

- Stream\<String> tokens() ==9==：

  产生一个字符串流，该字符串是调用这个扫描器的next方法时返回的

java.util.function.Supplier\<T> ==8==

- T get()：

  提供一个值

### 1.3、filter、map和flatMap方法

流的转换会产生新的流，它的元素派生自另一个流中的元素。

filter转换会产生一个新流，它的元素与某种条件相匹配

```java
List<String>  = ...;
Stream<String> longWords = words.stream().filter(w -> w.length() > 12);//将所有长度大于12的字符串放入流
```

filter的引元是Predicate\<T>，即从T到boolean的函数

map方法会将函数应用到每一个元素上，并且其结果是包含了应用该函数后所产生所有结果的流

```java
Stream<String> lowercaseWords = words.stream().map(String::toLowerCase);//将所有单词小写放入流
```

下面这个方法将字符串转换为字符集合再转为流

```java
public static Stream<String> codePoints(String s){
    ArrayList<String> result = new ArrayList<>();
    int i = 0;
    while (i < s.length()){
        int j = s.offsetByCodePoints(i, 1);
        result.add(s.substring(i,j));
        i = j;
    }
    return result.stream();
}
```

如果我们将codePoint方法映射到一个字符串流上，我们最终会的到一个包含流的流

```java
Stream<Stream<String>> result = words.stream().map(w -> codePoint(w));
```

这种情况，我们可以使用flatMap方法

```java
Stream<String> result = words.stream().flatMap(w -> codePoints(w));
```

java.util.stream.Stream\<T>==8==

- Stream\<T> filter(Predicate<? super T> predicate)：

  产生一个流，它包含当前流中所有满足谓词条件的元素

- \<R> Stream\<R> map(Function<? super T,? extends R> mapper) 

  产生一个流，它包含将mapper应用于当前流中所有元素所产生的结果

- \<R> Stream\<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper) ：

  产生一个流，它是通过将mapper应用于当前流中所有元素所产生的结果连接到一起而获得的。(注意，这里的每个结果都是一个流)

### 1.4、抽取子流和组合流

调用stream.limit(n)会返回一个新的流，它在n个元素之后结束(如果原来的流比n短，那么就会在该流结束时结束)。可以通过这个方法裁剪无限流的尺寸

```java
Stream<Double> limit = Stream.generate(Math::random).limit(100);
```

与之相反stream.skip(n)会丢弃前n个元素

```java
Stream<String> word = Stream.of(contents.split("\\PL+")).skip(1);//跳过第一个字符串
```

stream.takeWhile(predicate)调用会在谓词为true时获取流中的所有元素，然后停止

```java
Stream<String> stringStream = codePoints("13123").takeWhile(s -> "0123456789".contains(s));//如果字符串中只包含数字，将字符串放入流
```

dropWhile方法正相反，当谓词为true时丢弃元素，并产生一个由第一个是该条件为假的元素开始的所有元素构成的流

```java
Stream<String> stringStream = codePoints("12321qwe").dropWhile(s -> "0123456789".contains(s));//返回qwe流
```

concat是Stream的静态方法，可以将两个流合并在一起

```java
Stream<String> concat = Stream.concat(codePoints("324"), codePoints("sgs"));//返回324sgs流
```

java.util.stream.Stream\<T>==8==

- Stream\<T> limit(long maxSize) 
- Stream\<T> skip(long n) 
- default Stream\<T> takeWhile(Predicate<? super T> predicate) 
- default Stream\<T> dropWhile(Predicate<? super T> predicate) 
- static \<T> Stream\<T> concat(Stream<? extends T> a, Stream<? extends T> b) 

### 1.5、其他的流转换

distinct方法会返回一个流，它的元素是从原有流中产生的，即原来的元素按照同样的顺序剔除重复元素后产生。这些重复元素并不一定毗邻的。

```java
Stream<String> will = Stream.of("will", "will", "Wnfai", "2342", "will").distinct();
```

sorted方法用于排序，它有两种变体。一种用于操作Compatible元素的流，而另一种可以接受一个Comparator。

```java
Stream<String> longestFirst = words.stream().sorted(Comparator.comparing(String::length).reversed());//字符串由长到短排列
```

当我们对集合排序时可以不使用流。但是，当排序处理是流管道中的一部分时，sorted方法就会显得很有用

peek方法会产生另一个流，它的元素与原来流中的元素相同，但是在每次获取一个元素时，都会调用一个函数。

```java
Object[] objects = Stream.iterate(1.0, p -> p * 2).peek(e -> System.out.println("Fetching" + e)).limit(20).toArray();//获取2的19次幂，期间用peek打印结果
```

### 1.6、简单约简

约简是一种终结操作，它们会将流约简为可以在程序中使用的非流值。

count方法会返回流中元素的数量

max和min方法，分别返回最大值和最小值

findFrist返回的是非空集合中的第一个值。它通常在与filter组合使用时很有用。

findAny返回的是非空集合中的任意一个值，这个方法在并行处理流时很有效，因为流可以报告任何它找到的匹配而不是被限制为必须报告第一个匹配

以上方法返回的都是一个Optional\<T>类型的值

如果只想知道是否存在匹配，那么可以使用anyMatch。这个方法会接受一个断言引元，因此不需要使用filter。

allMatch和noneMatch方法，它们分别在所有元素和没有任何元素匹配谓词的情况下返回true。

java.util.stream.Stream\<T>

- Optional\<T> max(Comparator<? super T> comparator)
- Optional\<T> min(Comparator<? super T> comparator) 
- Optional\<T> findAny() 
- Optional\<T> findFirst() 
- boolean allMatch(Predicate<? super T> predicate) 
- boolean anyMatch(Predicate<? super T> predicate) 
- boolean noneMatch(Predicate<? super T> predicate) 

### 1.7、Optional类型

Optional\<T>对象是一种包装器对象，要么包装了类型T的对象，要么没有包装任何对象。

#### 1.7.1、获取Optional值

Optional在值不存在的情况下会产生一个可替代物，而只有在值存在的情况下才会使用这个值

第一条策略：在没有任何匹配时，我们会希望使用某种默认值，可能是空字符串

```java
String result = optionalString.orElse("");
```

可以调用代码来计算默认值：

```java
String result = optionalString.orElseGet(() -> System.getProperty("myapp.default"));
```

可以在没有任何值时抛出异常

```java
String result = optionalString.orElseThrow(IllegalStateExcption::new);
```

java.util.Optional\<T>

- T orElse(T other) 
- T orElseGet(Supplier<? extends T> supplier)
- T orElseThrow() 

#### 1.7.2、消费Optional值

第二条策略：只有在其存在的情况下才消费该值

ifPresent方法会接受一个函数。如果可选值存在，那么它会被传递给该函数。否则，不会发生任何事情

```java
optionalValue.ifPresent(v -> Prosess V);
```

如果想要在可选值存在是执行一种动作，在可选值不存在时执行另一种动作，可以使用ifPresentOrElse

```java
optionalValue.ifPresentOrElse(
	v -> System.out.println("Found" + v),
    () -> logger.warning("No Match!")
);
```

java.util.Optional\<T>

- void ifPresent(Consumer<? super T> action)
- void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction) ==9==

#### 1.7.3、管道化Optional值

第三条策略：保持Optional完整

使用map方法来转换Optional内部的值：

```java
Optional<String> transformed = optionalString.map(String::toUpperCase);
```

使用filter方法来只处理那些在转换它之前或之后满足某种特定属性的Optional值。如果不满足该属性，那么管道会产生空的结果。

```java
Optional<String> transformed = optionalString.filter(s -> s.length() > 8)
    .map(String::toUpperCase);
```

使用or方法将空Optional替换为一个可替代的Optional。这个可替代值将以惰性方式计算。

```java
Optional<String> transformed = optionalString.or(() -> alternatives.stream().findFirst());
```

java.util.Optional\<T>

- \<U> Optional\<U> map(Function<? super T,? extends U> mapper) 
- Optional\<T> filter(Predicate<? super T> predicate) 
- Optional\<T> or(Supplier<? extends Optional<? extends T>> supplier) ==9==

#### 1.7.4、不适合使用Optional值的方式

Optional类型正确用法提示：

- Optional类型的变量永远都不应该为null
- 不要使用Optional类型的域(所有成员变量)。因为其代价是额外多出来一个对象。在类的内部，使用null表示缺失的域更易于操作
- 不要在集合中放置Optional对象，并且不要将它们用作map的键。应该直接收集其中的值

java.util.Optional==8==

- T get()

- T ofelseThrow()==10==

  产生这个Optional的值，或者在该Optional为空时，抛出一个NosuchElementException异常

- boolean isPresent

  如果该Optional不为空，则返回true

#### 1.7.5、创建Optional的值

Optional有三个静态方法可以创建Optional对象，分别为if、ofNullable、empty

```java
public static Optional<Double> inverse(Double x){
    return x==0 ? Optional.empty() : Optional.of(1/x);
}
```

java.util.Optional==8==

- static \<T> Optional\<T> of(T value)

- static \<T> Optional\<T> ofNullable(T value)

  产生一个具有给定值的Optional。如果value为null，那么第一个方法会抛出一个NullPointerException异常，而第二个方法会产生一个空Optional

- static \<T> Optional\<T> empty():

  产生一个空Optional

#### 1.7.6、用flatMap构建Optional值的函数

假设你有一个方法f可产生Optional\<T>对象，并且目标类型T具有一个可产生Optional\<U>对象的方法g，如果它们都是普通方法，那么可以s.f().g()调用，但是这种组合无法工作，因为s.f()的类型为Optional\<T>，而不是T。因此可以使用以下方法

```java
Optional<U> result = s.f().flatMap(T::g);
```

如果s.f()的值存在，那么g就可以应用到它上面。否则，就会返回一个空Optional\<U>

java.util.Optional==8==

- \<U> Optional\<U> flatMap(Function<? super T,? extends Optional<? extends U>> mapper) 

  如果Optional存在，产生将mapper应用于当前Optional值所产生的结果，或者在当前Optional为空时，返回一个空Optional

#### 1.7.7、将Optional转换为流

stream方法会将一个Optional\<T>对象转换为一个具有0个或1个元素的Stream\<T>对象











```
<U> Optional<U> flatMap​(Function<? super T,​? extends Optional<? extends U>> mapper) 
```





