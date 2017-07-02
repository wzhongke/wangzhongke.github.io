title: Java Stream

---------------------
# Streams (Java 8以上)
`Stream`是一系列元素，并且支持在这些元素上进行不同的计算操作。Java8中，`Collection`接口有两个方法可以生成`Stream`： `stream()`和`parallelStream()`. `Stream`操作可以是中间操作或者是最终操作。中间操作会返回一个`Stream`，这样可以进行链式操作。最终操作返回void或者非stream结果。
## 使用Streams
流是可以执行顺序和并行聚合操作的一系列元素。任何给定的流可能会有无限量的数据流过它。因此，从流中接收到的数据在到达时被单独处理，而不是完全对数据及逆行批处理。结合lambda表达式，能够提供一个简洁的方式初拉力流中的数据
```
Stream<String> fruitStream = Stream.of("apple", "banan");
fruitStream.filter(s->s.contains("a")).map(String::toUpperCase).sorted().forEach(System.out::println);
```