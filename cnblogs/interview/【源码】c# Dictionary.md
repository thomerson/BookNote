<!-- https://zhuanlan.zhihu.com/p/471981620 -->

## Dictionary

对应java中的map

1. Entry结构体

```c#
 private struct Entry
 {
     public uint hashCode; 
     /// <summary>
     /// 0-based index of next entry in chain: -1 means end of chain
     /// also encodes whether this entry _itself_ is part of the free list by changing sign and subtracting 3,
     /// so -2 means end of free list, -3 means index 0 but on free list, -4 means index 1 but on free list, etc.
     /// </summary>
     public int next; //下一个元素的下标索引
     public TKey key;     // Key of entry
     public TValue value; // Value of entry
 }
```
是Dictionary中存放数据的最小单位

2. 变量

```c#

     private int[]? _buckets; //Hash桶
     private Entry[]? _entries; //Entry数组，用于存放元素
     private int _count; //当前Entries的index位置
     private int _freeList; // 被删除Entry在Entries中的下标index
     private int _freeCount; // 有多少被删除的Entry
     private int _version; // 当前版本，防止迭代过程中集合被更改
     private IEqualityComparer<TKey>? _comparer; // 比较器
     private KeyCollection? _keys; // 存放key的集合
     private ValueCollection? _values; // 存放Value的集合
     private const int StartOfFreeList = -3;

```


3. 构造器

```c#

 public Dictionary(int capacity, IEqualityComparer<TKey>? comparer)
 {
     if (capacity < 0)
     {
         ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.capacity);
     }
 ​
     if (capacity > 0)
     {
         Initialize(capacity);
     }
 ​
     if (comparer is not null && comparer != EqualityComparer<TKey>.Default) // first check for null to avoid forcing default comparer instantiation unnecessarily
     {
         _comparer = comparer;
     }
 ​
     // Special-case EqualityComparer<string>.Default, StringComparer.Ordinal, and StringComparer.OrdinalIgnoreCase.
     // We use a non-randomized comparer for improved perf, falling back to a randomized comparer if the
     // hash buckets become unbalanced.
     if (typeof(TKey) == typeof(string))
     {
         IEqualityComparer<string>? stringComparer = NonRandomizedStringEqualityComparer.GetStringComparer(_comparer);
         if (stringComparer is not null)
         {
             _comparer = (IEqualityComparer<TKey>?)stringComparer;
         }
     }
 }

 private int Initialize(int capacity)
 {
     int size = HashHelpers.GetPrime(capacity);
     int[] buckets = new int[size];
     Entry[] entries = new Entry[size];
 ​
     // Assign member variables after both arrays allocated to guard against corruption from OOM if second fails
     _freeList = -1;
     _buckets = buckets;
     _entries = entries;
 ​
     return size;
 }
```

设置size为大于容量的一个最小质数。

初始化int[] buckets，大小为size,用来进行Hash碰撞。

初始化Entry[] entries，大小为size，用来存储字典的内容，并且标识下一个元素的位置