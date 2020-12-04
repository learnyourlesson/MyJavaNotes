# 关于ArrayList和LinkedList以及TreeList的插入效率

直接放结论吧，我从别人的博客中看到的

1. 在尾部插入数据，数据量较小时LinkedList比较快，因为ArrayList要频繁扩容，当数据量大时ArrayList比较快，因为ArrayList扩容是当前容量*1.5，大容量扩容一次就能提供很多空间，当ArrayList不需扩容时效率明显比LinkedList高，因为直接数组元素赋值不需new Node

2. 在首部插入数据，LinkedList较快，因为LinkedList遍历插入位置花费时间很小，而ArrayList需要将原数组所有元素进行一次System.arraycopy

3. 插入位置越往中间，LinkedList效率越低，因为它遍历获取插入位置是从两头往中间搜，index越往中间遍历越久，因此ArrayList的插入效率可能会比LinkedList高

插入位置越往后，ArrayList效率越高，因为数组需要复制后移的数据少了，那么System.arraycopy就快了，因此在首部插入数据LinkedList效率比ArrayList高，尾部插入数据ArrayList效率比LinkedList高

LinkedList可以实现队列，栈等数据结构，这是它的优势



​	但是现在有新的 apache 提供的 TreeList，使用了树结构保证了所有的插入和删除均为O（log n）

按照文档中所讲，TreeList 基本可以完美替代 LinkedList ，只有在极少数情况下 LinkedList 会优于 TreeList （add添加到末尾），而ArrayList 在普通情况下仍然是和优秀的。

```
              get  add  insert  iterate  remove
 TreeList       3    5       1       2       1
 ArrayList      1    1      40       1      40
 LinkedList  5800    1     350       2     325
```

`ArrayList` is a good general purpose list implementation. It is faster than `TreeList` for most operations except inserting and removing in the middle of the list. `ArrayList` also uses less memory as `TreeList` uses one object per entry.

`LinkedList` is rarely a good choice of implementation. `TreeList` is almost always a good replacement for it, although it does use slightly more memory.

参考：
[小橘子2222](https://juejin.im/post/6844903790156447752)

[官方链接](https://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/list/TreeList.html)