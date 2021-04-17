## python字典实现方式

#### 哈希表 (hash tables)

哈希表（也叫散列表），根据键值对(Key-value)而直接进行访问的数据结构。它通过把key和value映射到表中一个位置来访问记录，这种查询速度非常快，更新也快。而这个映射函数叫做哈希函数，存放值的数组叫做哈希表。 哈希函数的实现方式决定了哈希表的搜索效率。具体操作过程是：

1. 数据添加：把key通过哈希函数转换成一个整型数字，然后就将该数字对数组长度进行取余，取余结果就当作数组的下标，将value存储在以该数字为下标的数组空间里。
2. 数据查询：再次使用哈希函数将key转换为对应的数组下标，并定位到数组的位置获取value。

但是，对key进行hash的时候，不同的key可能hash出来的结果是一样的，尤其是数据量增多的时候，这个问题叫做哈希冲突。如果解决这种冲突情况呢？通常的做法有两种，一种是链地址法（开散列），另一种是开放寻址法（闭散列），Python选择后者。

#### 开放寻址法（open addressing，也叫闭散列）

开放寻址法中，所有的元素都存放在散列表里，当产生哈希冲突时，通过一个探测函数计算出下一个候选位置，如果下一个获选位置还是有冲突，那么不断通过探测函数往下找，直到找个一个空槽来存放待插入元素。

探测下一个空位置的时候可以采用二次探测法：就是当有哈希冲突时，寻找下一个空闲位置时，首先在该位置处加1的平方，若加1的平方的位置处依然有元素，那就加2的平方，知道找到一个空闲的位置为止。

### 链地址法（开散列）

**先用哈希函数计算每个数据的散列地址，把具有相同地址的元素归于同一个集合之中，把该集合处理为一个链表，链表的头节点存储于哈希表之中。**
