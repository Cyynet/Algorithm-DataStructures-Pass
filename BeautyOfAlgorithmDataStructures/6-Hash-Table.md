# 6-散列表
散列表用的是数组支持按照下标随机访问数据的特性，所以散列表其实就是数组的一种扩展，由数组演化而来。可以说，如果没有数组，就没有散列表。

包括键（key）或者关键字。我们把key转化为数组下标的映射方法就叫作散列函数（或“Hash 函数”“哈希函数”），而散列函数计算得到的值就叫作散列值（或“Hash 值”“哈希值”）。

我总结了三点散列函数设计的基本要求：
- 散列函数计算得到的散列值是一个非负整数；
- 如果 key1 = key2，那 hash(key1) == hash(key2)；
- 如果 key1 ≠ key2，那 hash(key1) ≠ hash(key2)。

散列表之不可避免-**散列冲突**

解决散列冲突两种办法
- 开放寻址法：当我们往散列表中插入数据时，如果某个数据经过散列函数散列之后，存储位置已经被占用了，我们就从当前位置开始，依次往后查找，看是否有空闲位置，直到找到为止。线性探测方法之外，还有另外两种比较经典的探测方法，二次探测（Quadratic probing）和双重散列（Double hashing）。
- 链表法：在散列表中，每个“桶（bucket）”或者“槽（slot）”会对应一条链表，所有散列值相同的元素我们都放到相同槽位对应的链表中。

##### 两个思考题
假设我们有 10 万条 URL 访问日志，如何按照访问次数给 URL 排序？
有两个字符串数组，每个数组大约有 10 万条字符串，如何快速找出两个数组中相同的字符串？
[Answer](https://time.geekbang.org/column/article/64233)

##### 如何选择冲突解决方法？

当数据量比较小、装载因子小的时候，适合采用开放寻址法。这也是 Java 中的ThreadLocalMap使用开放寻址法解决散列冲突的原因。

基于链表的散列冲突处理方法比较适合存储大对象、大数据量的散列表，而且，比起开放寻址法，它更加灵活，支持更多的优化策略，比如用红黑树代替链表。那最终退化成的散列表的查找时间也只不过是 O(logn)。

##### 何为一个工业级的散列表？
几点要求：支持快速的查询、插入、删除操作；内存占用合理，不能浪费过多的内存空间；性能稳定，极端情况下，散列表的性能也不会退化到无法接受的情况。

##### 如何实现这样一个散列表呢？
设计一个合适的散列函数；定义装载因子阈值，并且设计动态扩容策略；选择合适的散列冲突解决方法。

##### 散列表+双向链表实现 LRU缓存淘汰算法

每个结点会在两条链中。一个链是刚刚我们提到的双向链表，另一个链是散列表中的拉链。前驱和后继指针是为了将结点串在双向链表中，hnext 指针是为了将结点串在散列表的拉链中。

![LRU](/BeautyOfAlgorithmDataStructures/Resources/6-Hash-Table-01.png)

首先，我们来看如何查找一个数据。我们前面讲过，散列表中查找数据的时间复杂度接近 O(1)，所以通过散列表，我们可以很快地在缓存中找到一个数据。当找到数据之后，我们还需要将它移动到双向链表的尾部。

其次，我们来看如何删除一个数据。我们需要找到数据所在的结点，然后将结点删除。借助散列表，我们可以在 O(1) 时间复杂度里找到要删除的结点。因为我们的链表是双向链表，双向链表可以通过前驱指针 O(1) 时间复杂度获取前驱结点，所以在双向链表中，删除结点只需要 O(1) 的时间复杂度。

最后，我们来看如何添加一个数据。添加数据到缓存稍微有点麻烦，我们需要先看这个数据是否已经在缓存中。如果已经在其中，需要将其移动到双向链表的尾部；如果不在其中，还要看缓存有没有满。如果满了，则将双向链表头部的结点删除，然后再将数据放到链表的尾部；如果没有满，就直接将数据放到链表的尾部。