
## HashTable

数组+链表

链表主要解决哈希冲突

![java-hashtable](https://img2023.cnblogs.com/blog/999484/202304/999484-20230403104257405-1919972934.png)


1. 属性

```c#

internal const Int32 HashPrime = 101;
private const Int32 InitialSize = 3;

private struct bucket {
    public Object key;//键
    public Object val;//值
    public int hash_col;//哈希码
}

private bucket[] buckets;
private int count;//元素总数
private int occupancy;//冲突次数

private  int loadsize; //装载容量值，相当于一个阈值，达到了这个数值，将对哈希表进行扩容
private  float loadFactor; // 哈希表中的元素占有数据桶空间的一个比率，这个比例直接决定了哈希表在什么时候进行扩容

private volatile int version;
private volatile bool isWriterInProgress;   

private ICollection keys;
private ICollection values;

```

* buckets

    数据桶，用于存储哈希表中的元素，它是一个结构体


```c#
private struct bucket
{
    public object? key; // 不能重复
    public object? val;  // 可以是任何的类型
    public int hash_coll;   // Store hash code; sign bit means there was a collision.
    // 它的最高位是符号位，为“0”时，表示这是一个正整数；为“1”时表示负整数。hash_coll使用最高位表示当前位置是否发生冲突，正数表示未发生冲突；负数表示当前位置存在冲突。之所以专门使用一个位用于存放哈希码并标注是否发生冲突，主要是为了提高哈希表的运行效率。
}
```

2. 构造函数

```c#

public Hashtable(int capacity, float loadFactor)
{
    if (capacity < 0)
        throw new ArgumentOutOfRangeException(nameof(capacity), SR.ArgumentOutOfRange_NeedNonNegNum);
    if (!(loadFactor >= 0.1f && loadFactor <= 1.0f))
        throw new ArgumentOutOfRangeException(nameof(loadFactor), SR.Format(SR.ArgumentOutOfRange_HashtableLoadFactor, .1, 1.0));

    // Based on perf work, .72 is the optimal load factor for this table.
    _loadFactor = 0.72f * loadFactor;

    double rawsize = capacity / _loadFactor;
    if (rawsize > int.MaxValue)
        throw new ArgumentException(SR.Arg_HTCapacityOverflow, nameof(capacity));

    // Avoid awfully small sizes
    int hashsize = (rawsize > InitialSize) ? HashHelpers.GetPrime((int)rawsize) : InitialSize;
    _buckets = new bucket[hashsize];

    _loadsize = (int)(_loadFactor * hashsize);
    _isWriterInProgress = false;
    // Based on the current algorithm, loadsize must be less than hashsize.
    Debug.Assert(_loadsize < hashsize, "Invalid hashtable loadsize!");
}


```

* capacity：最初可包含的元素的近似数目。
* loadFactor：0.1 到 1.0 范围内的数字，再乘以提供最佳性能的默认值```0.72f```。结果是元素与存储桶的最大比率，建议该值使用默认的1.0f，因为该值越小，越会造成空间的浪费


3. 构建Hash函数

```c#

private uint InitHash(Object key, int hashsize, out uint seed, out uint incr) {
    //取正数值,第一和哈希函数h_1(k)
    uint hashcode = (uint) GetHash(key) & 0x7FFFFFFF;
    seed = (uint) hashcode;
    //第二个哈希函数h_2(k)的增量
    incr = (uint)(1 + ((seed * HashPrime) % ((uint)hashsize - 1)));
    return hashcode;
}

```

<!-- https://blog.csdn.net/exiaojiu/article/details/51206024 -->