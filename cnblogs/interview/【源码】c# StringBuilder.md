## StringBuilder

对象 StringBuilder 维护一个缓冲区，以适应字符串的扩展。 如果空间可用，则会将新数据追加到缓冲区;否则，将分配更大的新缓冲区，将原始缓冲区中的数据复制到新缓冲区，然后将新数据追加到新缓冲区





1. 定义

```public sealed class StringBuilder : ISerializable```

此类不能被继承

2. 构造函数

```c#

public StringBuilder();
public StringBuilder(int capacity);
public StringBuilder(string? value);
public StringBuilder(int capacity, int maxCapacity);
public StringBuilder(string? value, int capacity);
public StringBuilder(string? value, int startIndex, int length, int capacity);
```

⭐默认capacity为**16**

⭐最大capacity为```Int32.MaxValue```

3. 添加拓展长度

```c#

using System;
using System.Reflection;
using System.Text;

public class Example
{
   public static void Main()
   {
      StringBuilder sb = new StringBuilder();
      ShowSBInfo(sb);
      sb.Append("This is a sentence.");
      ShowSBInfo(sb);
      for (int ctr = 0; ctr <= 10; ctr++) {
         sb.Append("This is an additional sentence.");
         ShowSBInfo(sb);
      }   
   }
   
   private static void ShowSBInfo(StringBuilder sb)
   {
      foreach (var prop in sb.GetType().GetProperties()) {
         if (prop.GetIndexParameters().Length == 0)
            Console.Write("{0}: {1:N0}    ", prop.Name, prop.GetValue(sb));
      }
      Console.WriteLine();
   }
}
// The example displays the following output:
//    Capacity: 16    MaxCapacity: 2,147,483,647    Length: 0
//    Capacity: 32    MaxCapacity: 2,147,483,647    Length: 19
//    Capacity: 64    MaxCapacity: 2,147,483,647    Length: 50
//    Capacity: 128    MaxCapacity: 2,147,483,647    Length: 81
//    Capacity: 128    MaxCapacity: 2,147,483,647    Length: 112
//    Capacity: 256    MaxCapacity: 2,147,483,647    Length: 143
//    Capacity: 256    MaxCapacity: 2,147,483,647    Length: 174
//    Capacity: 256    MaxCapacity: 2,147,483,647    Length: 205
//    Capacity: 256    MaxCapacity: 2,147,483,647    Length: 236
//    Capacity: 512    MaxCapacity: 2,147,483,647    Length: 267
//    Capacity: 512    MaxCapacity: 2,147,483,647    Length: 298
//    Capacity: 512    MaxCapacity: 2,147,483,647    Length: 329
//    Capacity: 512    MaxCapacity: 2,147,483,647    Length: 360

```

如果将字符添加到 StringBuilder 对象，则其长度将增加，直到它等于 属性的大小 StringBuilder.Capacity ，该属性定义对象可以包含的字符数。 如果添加的字符数导致对象的长度 StringBuilder 超过其当前容量，则分配新的内存，将属性的值 Capacity **加倍**，将新字符添加到 StringBuilder 对象，并调整其 Length 属性。 对象 StringBuilder 的额外内存是动态分配的，直到它达到 属性定义的 StringBuilder.MaxCapacity 值。 达到最大容量时，无法为 StringBuilder 对象分配更多内存，尝试添加字符或将其扩展超过其最大容量会 ArgumentOutOfRangeException 引发 或 OutOfMemoryException 异常。

## 参考

[StringBuilder 类](https://learn.microsoft.com/zh-cn/dotnet/api/system.text.stringbuilder?redirectedfrom=MSDN&view=net-7.0)