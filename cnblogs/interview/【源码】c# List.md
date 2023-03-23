## List源码分析


1. 构造器

```c#
public class List<T> : IList<T>, IList, IReadOnlyList<T>
{
    private const int DefaultCapacity = 4;

    internal T[] _items; // Do not rename (binary serialization)
    internal int _size; // Do not rename (binary serialization)
    private int _version; // Do not rename (binary serialization)
    private static readonly T[] s_emptyArray = new T[0];
    public List()
     {
        _items = s_emptyArray;
     }

    // Constructs a List with a given initial capacity. The list is
    // initially empty, but will have room for the given number of elements
    // before any reallocations are required.
    //
    public List(int capacity)
    {
        if (capacity < 0)
            ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.capacity, ExceptionResource.ArgumentOutOfRange_NeedNonNegNum);

        if (capacity == 0)
            _items = s_emptyArray;
        else
            _items = new T[capacity];
    }
}
```

⭐List的底层是由**数组**构造的
⭐初始化的长度是**4**

2. ```Add```

```c#
public void Add(T item)
{
    _version++;
    T[] array = _items;
    int size = _size;
    if ((uint)size < (uint)array.Length)
    {
        _size = size + 1;
        array[size] = item;
    }
    else
    {
        AddWithResize(item);
    }
}

```

⭐每次会**判断当前容量是否足够，如果足够放入即可，不够的话调用```AddWithResize(item)```**


```c#
private void AddWithResize(T item)
{
    Debug.Assert(_size == _items.Length);
    int size = _size;
    Grow(size + 1);
    _size = size + 1;
    _items[size] = item;
}
```

添加并且调整大小。其中调整大小的部分由Grow实现

```c#
private void Grow(int capacity)
{
    Debug.Assert(_items.Length < capacity);

    int newcapacity = _items.Length == 0 ? DefaultCapacity : 2 * _items.Length;

    // Allow the list to grow to maximum possible capacity (~2G elements) before encountering overflow.
    // Note that this check works even when _items.Length overflowed thanks to the (uint) cast
    if ((uint)newcapacity > Array.MaxLength) newcapacity = Array.MaxLength;

    // If the computed capacity is still less than specified, set to the original argument.
    // Capacities exceeding Array.MaxLength will be surfaced as OutOfMemoryException by Array.Resize.
    if (newcapacity < capacity) newcapacity = capacity;

    Capacity = newcapacity;
}
public int Capacity
{
    get => _items.Length;
    set
    {
        if (value < _size)
        {
            ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.value, ExceptionResource.ArgumentOutOfRange_SmallCapacity);
        }

        if (value != _items.Length)
        {
            if (value > 0)
            {
                T[] newItems = new T[value];
                if (_size > 0)
                {
                    Array.Copy(_items, newItems, _size);
                }
                _items = newItems;
            }
            else
            {
                _items = s_emptyArray;
            }
        }
    }
}
```

⭐如果初始不给List赋值大小的话默认大小为4，如果再频繁调用Add，每次new都会产生内存垃圾，给GC带来了负担。这里需要注意**每次扩容都是两倍**，并且扩容之后是通过调用```Array.Copy(_items, newItems, _size)```**从原数组拷贝生成到新数组**。


3. ```Remove```

```c#
public bool Remove(T item)
{
    int index = IndexOf(item);
    if (index >= 0)
    {
        RemoveAt(index);
        return true;
    }

    return false;
}
public int IndexOf(T item)
    => Array.IndexOf(_items, item, 0, _size);
public void RemoveAt(int index)
{
    if ((uint)index >= (uint)_size)
    {
        ThrowHelper.ThrowArgumentOutOfRange_IndexException();
    }
    _size--;
    if (index < _size)
    {
        Array.Copy(_items, index + 1, _items, index, _size - index);
    }
    if (RuntimeHelpers.IsReferenceOrContainsReferences<T>())
    {
        _items[_size] = default!;
    }
    _version++;
}
```

删除的原理其实就是元素覆盖，通过```Array.IndexOf```找到元素索引，之后调用```Array.Copy```覆盖被删除的元素。

4. ```Insert```

```c#

public void Insert(int index, T item)
{
    // Note that insertions at the end are legal.
    if ((uint)index > (uint)_size)
    {
        ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.index, ExceptionResource.ArgumentOutOfRange_ListInsert);
    }
    if (_size == _items.Length) Grow(_size + 1);
    if (index < _size)
    {
        Array.Copy(_items, index, _items, index + 1, _size - index);
    }
    _items[index] = item;
    _size++;
    _version++;
}

```

⭐先检查容量是否足够，之后向后覆盖，接下来将元素放入即可

5. ```Clear```

```c#

public void Clear()
{
    _version++;
    if (RuntimeHelpers.IsReferenceOrContainsReferences<T>())
    {
        int size = _size;
        _size = 0;
        if (size > 0)
        {
            Array.Clear(_items, 0, size); // Clear the elements so that the gc can reclaim the references.
        }
    }
    else
    {
        _size = 0;
    }
}

```

⭐```Array.Clear```需要注意这不是删除方法，他是**将数组中每个元素重置为元素的默认值**。 它将引用类型的元素设置 (包括string) 到的元素 null ，并将值类型的元素设置为默认值。

6. ```Contains```

```c#
public bool Contains(T item)
{
    // PERF: IndexOf calls Array.IndexOf, which internally
    // calls EqualityComparer<T>.Default.IndexOf, which
    // is specialized for different types. This
    // boosts performance since instead of making a
    // virtual method call each iteration of the loop,
    // via EqualityComparer<T>.Default.Equals, we
    // only make one virtual call to EqualityComparer.IndexOf.

    return _size != 0 && IndexOf(item) != -1;
}
```

通过```IndexOf```来寻找元素

7. ```ToArray```

```c#
public T[] ToArray()
{
    if (_size == 0)
    {
        return s_emptyArray;
    }

    T[] array = new T[_size];
    Array.Copy(_items, array, _size);
    return array;
}
```

new一个新的数组，拷贝一份返回

8. ```Sort```

```c#
public void Sort(int index, int count, IComparer<T>? comparer)
{
    if (index < 0)
    {
        ThrowHelper.ThrowIndexArgumentOutOfRange_NeedNonNegNumException();
    }

    if (count < 0)
    {
        ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.count, ExceptionResource.ArgumentOutOfRange_NeedNonNegNum);
    }

    if (_size - index < count)
        ThrowHelper.ThrowArgumentException(ExceptionResource.Argument_InvalidOffLen);

    if (count > 1)
    {
        Array.Sort<T>(_items, index, count, comparer);
    }
    _version++;
}
```

调用```Array.Sort<T>(_items, index, count, comparer)```进行排序

> .NET Framework 4 及更早版本仅使用 Quicksort 算法

⭐Sort函数不是单纯的快速排序,此方法使用的是**内省sort(introsort)算法**

* 如果分区大小小于或等于16个元素，则它将使用 **插入排序** 算法

* 如果分区超过2*log^n,其中n为输入数组的范围，则使用则使用 Heapsort 算法(**堆排序**)。

* 否则，它使用的是**快速排序** 算法

9. 索引器

```c#

public T this[int index]
{
    get
    {
        // Following trick can reduce the range check by one
         if ((uint)index >= (uint)_size)
         {
             ThrowHelper.ThrowArgumentOutOfRange_IndexException();
         }
        return _items[index];
    }

    set
    {
        if ((uint)index >= (uint)_size)
        {
           ThrowHelper.ThrowArgumentOutOfRange_IndexException();
        }
        _items[index] = value;
        _version++;
    }
}
```

使用数组索引获取元素

10. 迭代器

```c#
public interface IEnumerable
{
  IEnumerator GetEnumerator();
}

public interface IEnumerator
{
    bool MoveNext();

    object Current { get; }

    void Reset();
}
```



### 总结

* 内部还是用数组实现，只是由于数组是固定长度的，所以需要不断扩容操作，带来性能上的问题，初始化时可以先定义list长度，避免不断扩容

* list是线程不安全的，避免多线程操作list

* remove和insert都会出现内存整理操作，内存消耗比较大

* 内省排序

