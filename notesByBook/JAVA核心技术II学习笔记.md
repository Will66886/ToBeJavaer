# Core Java Volume II Study Notes

## 第一章、Java8的流库

### 1.1、从迭代到流的操作

流与集合的区别：

1. 流并不存储其元素。这些元素可能存储在底层的集合中，或者是按需生成的。
2. 流的操作不会修改器数据源。
3. 流的操作是尽可能惰性执行的。这意味着直至需要其结果时，操作才会执行。

java.util.stream.Stream\<T>==8==

- Stream\<T> filter(Predicate<? super T> p)：

  产生一个流，其中包含当前流中满足p的所有元素

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

如果想要按照某种方式来转换流中的值，可以使用map方法并传递执行该转换函数

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

假设我们有一个用户ID流和下面的方法

```java
Optional<User> lookup(String id);
```

想要获取用户流时，跳过那些无效的ID

```java
Stream<String> ids = ...;
Stream<User> users = ids.map(Users::lookup)
    .filter(Optional::isPresent)
    .map(Optional::get);
```

但是这么做更麻烦也更危险，我们应该慎用Optional的isPresent和get方法，如果ds==null，将会产生异常

```java
Stream<User> users = ids.map(Users::lookup)
    .filter(Optional::stream);
```

java.util.Optional

- \<U> Optional\<U> flatMap(Function<? super T,Optional\<U> mapper>) ==9==

  产生将mapper应用于当前Optional值所产生的结果，或者在当前Optional为空时，返回一个空Optional

### 1.8、收集结果

当处理完流之后，如果想要查看其结果。此时可以调用iterator方法，它会产生用来访问元素的旧式风格的迭代器。

也可以调用forEach方法，将某个函数应用于每个元素

```java
stream.forEach(System.out::print);
```

在并行流上，forEach方法会以任意顺序遍历各个元素。如果想要按照顺序来处理它们，可以调用forEachOrdered方法，当然，这样会使得并行流失去意义。

如果想要将结果收集到数据结构中，可以调用toArray，获得由流的元素构成的数组

由于无法在运行时创建泛型数组，所以直接调用的话，只能获得一个Object[]数组，如果想要让数组具有正确的类型，可以将其传到数组构造器中

```java
String[] result = stream.toArray(String[]::new);
```

可以使用collect方法，将流保存在一个Collector接口的实例中。收集器是一种收集众多元素并产生单一结果的对象，Collectors类提供了大量用于生成常见收集器的工厂方法。

```java
//收集到List集合
List<String> result = stream.collect(Collectors.toList());
//收集到Set集合
Set<String> result = stream.collect(Collectors.toSet());
//收集到TresSet
TreeSet<String> result = stream.collect(Collectors.toCollection(TreeSet::new));
//收集到String字符串
String result = stream.collect(Collectors.joining());
//收集到String字符串,并用逗号分隔
String result = stream.collect(Collectors.joining(","));
//如果stream中有除字符串以外的其他对象，可先将其转为String类型
String result = stream.map(Object::toString).collect(Collectors.joining(","));

//可通过summarizingInt方法，同时计算总和、数量、平均值、最大值和最小值
IntSummaryStatistics summary = stream.collect(Collectors.summarizingInt(String::length));
double averageWordLength = summary.getAverage();
int maxWordLength = summary.getMax();
```

java.util.stream.BaseStream ==8==

- Iterator\<T> iterator() 

  产生一个用于获取当前流中各个元素的迭代器。这是一种终结操作。

java.util.stream.Stream ==8==

- void forEach(Consumer<? super T> action) 

  在流的每个元素上调用action。这是一种终结操作

- Object[] toArray() 

- \<A> A[] toArray(IntFunction<A[]> generator) 

  产生一个对象数组，或者在将引用A[]::new传递给构造器时，返回一个A类型的数组。这些操作都是终结操作

- <R,A> R collect(Collector<? super T,A,R> collector) 

  使用给定的收集器来收集当前流中的元素。Collectors类有用于多种收集器的工厂方法

java.util.stream.Collectors ==8==

- static \<T> Collector<T,?,List\<T>> toList()

- static \<T> Collector<T,?,List\<T>> toUnmodifiableList() ==10==

- static \<T> Collector<T,?,Set\<T>> toSet()

- static \<T> Collector<T,?,Set\<T>> toUnmodifiableSet()  ==10==

  产生一个将元素收集到列表或集合中的收集器

- static <T,C extends Collection\<T>> Collector<T,?,C> toCollection(Supplier\<C> collectionFactory) 

  产生一个将元素收集到任意集合中的收集器。可以传递一个诸如TreeSet::new的构造引用

- static <T,C extends Collection\<T>> Collector<T,?,C> toCollection(Supplier\<C> collectionFactory) static Collector<CharSequence,?,String> joining()

- static Collector<CharSequence,?,String> joining(CharSequence delimiter)

- static Collector<CharSequence,?,String> joining(CharSequence delimiter, CharSequence prefix, CharSequence suffix)

  产生一个连接字符串的收集器。分隔符会置于字符串之间，而第一个字符串之前可以有前缀，最后一个字符串之后可以有后缀。如果没有指定，那么它们都为空

- static \<T> Collector<T,?,DoubleSummaryStatistics> summarizingDouble(ToDoubleFunction<? super T> mapper)

- static \<T> Collector<T,?,IntSummaryStatistics> summarizingInt(ToIntFunction<? super T> mapper)

- static \<T> Collector<T,?,LongSummaryStatistics> summarizingLong(ToLongFunction<? super T> mapper)

  产生能够生成(Int|Long|Double)SummaryStatistics对象的收集器，通过它们可以获得将mapper应用于每个元素后所产生的结果的数量、总和、平均值、最大值和最小值

IntSummaryStatistics ==8==
LongSummaryStatistics ==8==
DoubleSummaryStatistics ==8==

- long getCount()：产生汇总后的元素的个数

- (int|long|double) getSum

- double getAvagerge()

  产生汇总后的元素的总和或平均值，或者在没有任何元素时返回0。

- (int|long|double) getMax()

- (int|long|double) getMin()

  产生汇总后的元素的最大值和最小值，或者在没有任何元素时，产生(Integer|Long|Double).(MAX|MIN)_VALUE。

### 1.9、收集到映射表中

Collectors.toMap方法可以将Stream中的元素收集到一个映射表中，这个方法有两个函数引元，它们用来产生映射表中的键和值

```java
Map<Integer, String> idToName = people().collect(
    Collectors.toMap(Person::getId, Person::getName));
```

如果有多个元素具有相同的键，就会产生冲突，收集器将会抛出一个IllegalStateException异常。可以通过提供第三个函数引元来覆盖这种行为，该函数会针对给定的已有值和新增值来解决冲突并确定键对应的值。这个函数应该返回已有值、新值或它们的组合

```java
Map<Integer, Person> idToPerson = people().collect(
    Collectors.toMap(Person::getId, Function.identity(),
                     (existingValue, newValue) -> newValue));
```

如果想要得到TreeMap，那么可以将构造器作为第4个引元来提供。你必须提供一种合并函数

```java
TreeMap<Integer, Person> idToPerson = people().collect(Collectors.toMap(Person::getId, Function.identity(),
        (existingValue, newValue) -> {
            throw new IllegalStateException();},
        TreeMap::new));
```

### 1.10、群组和分区

groupingBy可以将具有相同特性的值群聚成组

```java
locales = Stream.of(Locale.getAvailableLocales());
Map<String, List<Locale>> countyToLocaleSet = locales.collect(groupingBy(Locale::getCountry));
```

当分类函数式断言函数(即返回是boolean值的函数)时，流的元素可以分为两个列表：该函数返回true的元素和其他的元素。在这种情况下，使用partitioningBy比使用groupingBy更高效。

```java
Map<Boolean, List<Locale>> englishAndOtherLocales = locales
    .collect(partitioningBy(l -> l.getLanguage().equals("en")));
System.out.println(englishAndOtherLocales.get(true));
```

java.util.stream.Collectors ==8==

- static <T,K> Collector<T,?,Map<K,List\<T>>> groupingBy(Function<? super T,? extends K> classifier) 

- static <T,K> Collector<T,?,ConcurrentMap<K,List\<T>>> groupingByConcurrent(Function<? super T,? extends K> classifier) 

  产生一个收集器，它会产生一个映射表或并发映射表，其键是将classifer应用于所有收集到的元素上所产生的结果，而值是由具有相同键元素构成的一个个列表

- static \<T> Collector<T,?,Map<Boolean,List\<T>>> partitioningBy(Predicate<? super T> predicate) 

  产生一个收集器，它会产生一个映射表，其键是true/false，而值是由满足/不满足断言的元素构成的列表

### 1.11、下游收集器

groupingBy方法会产生一个映射表，它的每个值都是一个列表。如果想要以某种方式来处理这些列表，就需要一个“下游收集器”，比如上个例子中toSet就是一种下游收集器

counting会产生收集到元素的个数

```java
Map<String, Long> countyToLocaleCounts = locales.collect(groupingBy(
        Locale::getCountry, counting()));
```

summing(Int|Long|Double)会接受一个函数作为引元，将该函数应用到下游元素中，并产生它们的和

```java
Stream<City> cities = readCities("src\\com\\test\\cities.txt");
Map<String, Integer> stateToCityPopulation = cities.collect(groupingBy(City::getName, summingInt(City::getPopulation)));
```

maxBy和minBy会接受一个比较器，并分别产生下游元素中的最大值和最小值

```java
Map<String, Optional<String>> stateToLongestCityName = cities
        .collect(groupingBy(City::getState,
                mapping(City::getName, maxBy(Comparator.comparing(String::length)))));
```

collectingAndThen收集器在收集器后面添加一个最终处理步骤

````java
Map<Character, Integer> stringCountsByStartingLetter = strings.collect(
    groupingBy(s -> s.charAt(0),
               collectingAndThen(toSet(), Set::size)));
````

mapping收集器会将一个函数应用于收集到的每个元素，并将结果传递给下游收集器

```java
Map<Object, Set<Integer>> stringCountsByStartingLetter = strings.collect(
    groupingBy(s -> s.charAt(0),
               mapping(String::length,toSet())));
```

最好使用groupingBy和partitioningBy一起处理“下游的”映射表中的值。否则，应该直接在流上应用诸如map、reduce、count、max或min这样的方法。

java.util.stream.Collectors ==8==

- static <T,K,D,A,M extends Map<K,D>> Collector<T,?,M> groupingBy(Function<? super T,? extends K> classifier, Supplier\<M> mapFactory, Collector<? super T,A,D> downstream) 

  产生一个收集器，该收集器会产生一个映射表，其中的键是将classifier应用到所有收集器的元素上之后产生的结果，而值是使用下游收集器收集具有相同键的元素而产生的结果

- static \<T> Collector<T,?,Long> counting() 

  产生一个可以对收集到的元素进行计数的收集器

- static \<T> Collector<T,?,Double> summingDouble(ToDoubleFunction<? super T> mapper)

- static \<T> Collector<T,?,Integer> summingInt(ToIntFunction<? super T> mapper)

- static \<T> Collector<T,?,Long> summingLong(ToLongFunction<? super T> mapper) 

  产生一个收集器，对将mapper应用到收集到的元素上之后产生的结果计算总和。

- static \<T> Collector<T,?,Optional\<T>> maxBy(Comparator<? super T> comparator)

- static \<T> Collector<T,?,Optional\<T>> minBy(Comparator<? super T> comparator) 

  产生一个收集器，使用comparator指定的排序方法，计算收集到的元素中的最大值和最小值

- static <T,A,R,RR> Collector<T,A,RR> collectingAndThen(Collector<T,A,R> downstream, Function<R,RR> finisher) 

  产生一个收集器，它会将元素发送到下游收集器中，然后将finisher函数应用到其结果上

- static <T,U,A,R> Collector<T,?,R> mapping(Function<? super T,? extends U> mapper, Collector<? super U,A,R> downstream) 

  产生一个收集器，它会在每个元素上调用mapper，并将结果发送到下游收集器中。

- static <T,U,A,R> Collector<T,?,R> flatMapping(Function<? super T,? extends Stream<? extends U>> mapper, Collector<? super U,A,R> downstream) 

  产生一个收集器，它会在每个元素上调用mapper，并将结果中的元素发送到下游收集器

- static <T,A,R> Collector<T,?,R> filtering(Predicate<? super T> predicate, Collector<? super T,A,R> downstream) 

  产生一个收集器，它会将满足谓词逻辑的元素发送到下游收集器中

### 1.12、约简操作

reduce方法是一种用于从流中计算某个值的通用机制，其最简单的形式将接受一个二元函数，并从两个元素开始持续应用它

```java
//将values中的所有值相加
List<Integer> values = ...;
Optional<Integer> sum = values.stream().reduce((x, y) -> x + y);
```

如果流为空，那么该方法会返回一个Optional，因为没有任何有效的结果

如果要使用并行流来约简，那么这项约简操作必须是可结合的，即组合元素时使用的顺序不会产生任何影响

幺元值：集合中进行运算但不会影响原集合的值，如0-10相加，那么0就是幺元值，1-100相乘，那么1就是幺元值

通常，会有一个幺元值e使得e op x = x，可以使用这个元素作为计算的起点，这样我们就可以使用第二种reduce

```java
//将values中所有值相加，如果values为空，则返回0
List<Integer> values = ...;
Integer sum = values.stream().reduce(0, (x, y) -> x + y);
```

如果流为空，则返回幺元值，你就再也不需要处理Optional类了

假设有一个对象流，并且想要对对象的某些属性求和，就不能使用上面的两种reduce

```java
Stream<String> words = ...;
```

首先，需要提供一个“累积器”函数(total,word) -> total + word.length()。这个函数会被反复调用，产生累积的总和。但是，当计算被并行化时，会有多个这种类似的计算。你需要将它们的结果合并。因此，你需要第二个函数来执行此处理

```java
int result = words.reduce(
0,
    (total, words) -> total + word.length(),
    (total, total2) -> total + total2
);
```

java.util.stream ==8==

- Optional\<T> reduce(BinaryOperator\<T> accumulator) 

- T reduce(T identity, BinaryOperator\<T> accumulator) 

- \<U> U reduce(U identity, BiFunction<U,? super T,U> accumulator, BinaryOperator\<U> combiner) 

  用给定的accumulator函数产生流中元素的累积总和。如果提供了幺元，那么第一个被累积的元素就是该幺元。如果提供了组合器，那么它可以用来将分别累积的各个部分整合成总和

- \<R> R collect(Supplier\<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner) 

  将元素收集到类型R的结果中。在每个部分上，都会调用supplier来提供初始结果，调用accumulator来交替地讲元素添加到结果中，并调用combiner来整合两个结果

### 1.13、基本类型流

流库中具有专门的类型IntStream、LongStream、DoubleStream。用来直接存储基本类型值

创建IntStream，需要调用IntStream.of和Arrays.stream方法

```java
IntSteam stream = IntStream.of(1,2,3,5);
stream = Arrays.stream(values, from, to);
```

也可以使用静态方法generate和iterate方法。此外IntStream和LongStream有静态方法range和rangeClosed，可以生成步长为1的整数范围

```java
IntStream is1 = IntStream.range(0,100);//0-99,Upper bound is excluded
IntStream is2 = IntStream.rangeClosed(0,100)//0-100,Upper bound is included
```

CharSequence接口拥有codePoints和chars方法，可以生成由字符的Unicode码或由UTF-16编码机制的码元构成的IntStream

```java
var sentence = "\uD835\uDD46 is the set of octonions.";
IntStream codes = sentence.codePoints();
```

当你有一个对象流时，可以用mapToInt、mapToLong或mapToDouble将其转换为基本类型流。

```java
Stream<String> words = ...;
IntStream lengths = words.mapToInt(String::length);
```

为了将基本类型流转换为对象流，需要使用boxed方法

```java
Stream<Integer> integers = IntStream.range(0,100).boxed();
```

基本类型流与对象流差异

- toArray方法会返回基本类型数组
- 产生可选结果的方法会返回一个OptionalInt、OptionalLong、OptionalDouble。这些类与Optional类类似，但是具有getAsInt、getAsLong和getAsDouble方法，而不是get方法
- 具有分别返回总和、平均值、最大值和最小值的sum、average、max和min方法。对象流没有定义这些方法
- summaryStatistics方法会产生一个类型为IntSummaryStatistics、LongSummaryStatistics或DoubleSummaryStatistics的对象，它们可以同时报告流的总和、数量、平均值、最大值和最小值

java.util.stream.IntStream ==8==

- static IntStream range(int startInclusive, int endExclusive) 

- static IntStream rangeClosed(int startInclusive, int endInclusive)

  产生一个由给定范围内的整数构成的IntStream 

- static IntStream of(int... values) 

  产生一个由给定元素构成的IntStream

- int[] toArray() 

  产生一个由当前流中的元素构成的数组

- int sum() 

- OptionalDouble average() 

- OptionalInt max() 

- OptionalInt min() 

- IntSummaryStatistics summaryStatistics() 

  产生当前流中元素的总和、平均值、最大值和最小值，或者产生一个可以从中获取所有这四个值的对象

- Stream\<Integer> boxed() 

  产生用于当前流中的元素的包装器对象流

java.util.stream.LongStream ==8==

- static LongStream range(long startInclusive, long endExclusive) 

- static LongStream rangeClosed(long startInclusive, long endInclusive)

  产生一个由给定范围内的整数构成的LongStream 

- static LongStream of(long ... values) 

  产生一个由给定元素构成的LongStream 

- long[] toArray() 

  产生一个由当前流中的元素构成的数组

- long sum() 

- OptionalDouble average() 

- OptionalLong max() 

- OptionalLong min() 

- LongSummaryStatistics summaryStatistics() 

  产生当前流中元素的总和、平均值、最大值和最小值，或者产生一个可以从中获取所有这四个值的对象

- Stream\<Long> boxed() 

  产生用于当前流中的元素的包装器对象流

java.util.stream.DoubleStream ==8==

- static DoubleStream of(double ... values) 

  产生一个由给定元素构成的DoubleStream 

- double [] toArray() 

  产生一个由当前流中的元素构成的数组

- double sum() 

- OptionalDouble average() 

- OptionalDouble max() 

- OptionalDouble min() 

- DoubleSummaryStatistics summaryStatistics() 

  产生当前流中元素的总和、平均值、最大值和最小值，或者产生一个可以从中获取所有这四个值的对象

- Stream\<Double> boxed() 

  产生用于当前流中的元素的包装器对象流

java.lang.CharSequence 

- default IntStream codePoints() ==8==

  产生由当前字符串的所有Unicode码点构成的流

java.util.Random

- IntStream ints()==8==

- IntStream ints(int randomNumberOrigin, int randomNumberBound)==8==

- IntStream ints(long streamSize)==8==

- IntStream ints(long streamSize, int randomNumberOrigin, int randomNumberBound)==8==

- LongStream longs()==8==

- LongStream longs(long streamSize)==8==

- LongStream longs(long randomNumberOrigin, long randomNumberBound)==8==

- LongStream longs(long streamSize, long randomNumberOrigin, long randomNumberBound)==8==

- DoubleStream doubles() ==8==

- DoubleStream doubles(double randomNumberOrigin, double randomNumberBound)==8==

- DoubleStream doubles(long streamSize)==8==

- DoubleStream doubles(long streamSize, double randomNumberOrigin, double randomNumberBound)==8==

  产生随机数流。如果提供了streamSize，这个流就是具有给定数量元素的有限流。当提供边界时，其元素将位于randomNumberOrigin(包括)和randomNumberBound(不包括)的区间内

java.util.Optional(Int | Long | Double) ==8==

- static Optional(Int | Long | Double)  of((int | long | double)  value) 

  用所提供的基本类型值产生一个可选择对象。

- (int | long | double)  getAs(Int | Long | Double) () 

  产生当前可选择对象的值，或者在其为空时抛出一个NoSuchElementException异常

- (int | long | double)  orElse(int other) 

- (int | long | double)  orElseGet((Int | Long | Double) Supplier supplier) 

  产生当前可选对象的值，或者在这个对象为空时产生可替代的值

- void ifPresent((Int | Long | Double) Consumer action) 

  如果当前可选对象不为空，则将其值传递给consumer

java.util.IntSummaryStatistics==8==

- double getAverage()

- long getCount()

- int getMax()

- int getMin()

- long getSum()

  产生收集到元素的数量、总和、平均值、最大值和最小值

### 1.14、并行流

Collection.parallelStream()方法可以从任何集合中获取一个并行流：

```java
Stream<String> parallelWords = words.parallelStream();
```

parallel方法可以将任意的顺序流转换为并行流

```java
Stream<String> parallelWords = Stream.of(wordArray).parallel();
```

只要在终结方法执行时流处于并行模式，所有的中间流操作都将被并行化。

当流操作并行运行时，其目标是让其返回结果与顺序执行时返回的结果相同，重要的是这些操作是无状态的，并且可以任意顺序执行

```java
int[] shortWords = new int[10];
//统计单词长度为1-9的个数
wordList.parallelStream().forEach(s -> {
    if (s.length() < 10) shortWords[s.length()]++;
});
```

上面这行代码，传递给forEach的函数会在多个并发线程中运行。相当于多个线程进行统计，并更新shortWords数组，但是这样会产生竞争情况，不同线程可能在竞争锁的时候导致一些单词遗漏统计

所以要确保传递给并行流操作的任何函数都可以安全地并行执行，而最佳方式是远离易变性

```java
Map<Integer, Long> shortWordCounts = wordList.parallelStream()
    .filter(s -> s.length() < 10)
    .collect(groupingBy(String::length, counting()));
```

上面代码，通过分组的方式，让每个线程分别统计不同长度单词的个数，这样就可以安全地并行化这些统计

可以通过放弃排序，来提高并行化的效率，通过在流上调用Stream.unrodered方法，就可以明确表示我们对排序不感兴趣。

```java
Stream.of("will", "will", "Wnfai", "2342", "will").distinct();
words.parallelStream().unrordered().limit(n);
```

Stream.distinct()就是通过放弃排序来提高效率的，也可以通过放弃排序来提高limit的效率

Collectors.groupingByConcurrent方法使用了共享的并发映射表，这样可以提高并发后合并映射表的效率，但是这样映射表中的值的顺序不会与流中的顺序相同

```java
ConcurrentMap<Integer, List<String>> result = wordList.parallelStream().collect(groupingByConcurrent(String::length));
```

如果使用独立于排序的下游收集器就不必在意排序问题了

```java
ConcurrentMap<Integer, Long> wordCounts = wordList.parallelStream().collect(groupingByConcurrent(String::length, counting()));
```

使用并行流时需要考虑以下几点

- 并行化会提高效率但也会导致大量的开销，只有面对非常大的数据集才划算
- 只有在底层的数据源可以被有效地分割为多个部分时，将流并行化才有意义
- 并行流使用的线程池可能会因诸如文件I/O或网络访问这样的操作被阻塞而饿死

默认情况下，并行流使用的是ForkJoinPool.commonPool返回的全局fork-join池，但是只有在不阻塞并且不与其他操作共享这个线程池时，才不会出问题。可以把操作放到定制的池的submit方法中

```java
ForkJoinPool customPool = . . .;
customPool.submit(() -> 
	stream.parallel().map(...).collect(...).get());
```

或者使用异步的方式

```java
ComletableFuture.supplyAsync(() -> 
	stream.parallel().map(...).collect(...),
    customPool).thenAccept(result -> ...);
```

Random.ints、Random.longs或者Random.doubles方法中获得的流不可分割。应该使用SplitableRandom类的ints、longs或doubles

java.util.stream.BaseStream<T,S extends BaseStream<T,S>> ==8==

- S parallel() 

  产生一个与当前流中元素相同的并行流

- S unordered() 

  产生一个与当前流中元素相同的无序流

java.util.Collection\<E>

- default Stream\<E> parallelStream() 

  用当前集合中的元素产生一个并行流

## 第二章、输入与输出









