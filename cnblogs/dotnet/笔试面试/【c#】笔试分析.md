## ```String s=new String("xyz")``` 创建了几个对象

常见的回答

```两个对象，一个是“xyz”,一个是指向“xyz”的引用对象s```

⭐分析

```String ```对象不可变，每次使用 ```System.String``` 类中的方法之一，都要在内存中新建字符串对象，这就需要为新对象分配新空间。

对于通过new产生一个字符串（"xyz"）时，会先去常量池中查找是否已经有了"xyz"对象，如果没有则在常量池中创建一个此字符串对象，然后堆中再创建一个常量池中此xyz对象的拷贝对象

## 字符串中```string str=null```和```string str=""```和```string str=string.Empty```的区别

## C# 中 string.Empty、""、null的区别 

* null很好理解，指定义一个对象，但是没有给该对象分配空间，即没有实例化该对象

此时只在栈上分配了空间，在堆上没有分配

* ""也很好理解，在栈上保存一个地址,这个地址占4字节，**指向内存堆中的某个长度为0的空间，这个空间保存的是string.Empty的实际值**

* string.Empty 是 C# 对 "" 在语法级别的优化

```c#
public static readonly string Empty; 
```


CLR 会维护一个字符串池，以防在堆中创建重复的字符串。而 string.Empty 是一种 C# 语法级别的优化，是在C#编译器将代码编译为 IL (即 MSIL )时进行了优化，**即所有对string类的静态字段Empty的访问都会被指向同一引用，以节省内存空间**。


