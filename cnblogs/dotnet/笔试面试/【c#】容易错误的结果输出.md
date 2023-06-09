## static readonly 和 const

先看代码

```c#

class ReadonlyConstDemo
{
    public const int a = b * 5;
    public const int b = 5;

    public static readonly int c = d * 5;
    public static readonly int d = 5;

}

Console.WriteLine($"a:{ReadonlyConstDemo.a},b:{ReadonlyConstDemo.b},c:{ReadonlyConstDemo.c},d:{ReadonlyConstDemo.d}");

```

结果

```a:25,b:5,c:0,d:5```

改变一下

```c#

class ReadonlyConstDemo
{
    static ReadonlyConstDemo()
    {
        c = d * 5;
    }

    public const int a = b * 5;
    public const int b = 5;

    public static readonly int c = 1;
    public static readonly int d = 5;

}

Console.WriteLine($"a:{ReadonlyConstDemo.a},b:{ReadonlyConstDemo.b},c:{ReadonlyConstDemo.c},d:{ReadonlyConstDemo.d}");

```

结果

```a:25,b:5,c:25,d:5```


⭐分析

* const修饰的常量在声明的时候必须初始化，
* readonly修饰的常量则可以**延迟到构造函数中初始化**

* const修饰的常量在编译期间就被解析，即常量值被替换成初始化的值，
* readonly修饰的常量则延迟到运行的时候

## override和new重写和覆盖

先看代码

```c#
abstract class A
{
    public virtual void Test()
    {
        Console.WriteLine("a");
    }
}

class B : A
{
    public override void Test()
    {
        Console.WriteLine("b");
    }
}


class C : A
{
    public new void Test()
    {
        Console.WriteLine("c");
    }
}


A a = new B();
A c = new C();

a.Test();
c.Test();

```

结果

```
b
a
```

⭐分析

* override:

    * 加上override关键字的属性或函数将完全覆盖基类的同名虚属性或虚函数，使基类的虚属性和虚函数在整个继承链中都不可见（在子类中用base关键字调用除外）
    
    * 父类和子类看到的都是子类override后的方法

* new

    * 表示**隐藏**，是指加上new关键字的属性或函数将对本类和继承类隐藏基类的同名属性或函数
    
    * 父类看不到子类的new的新方法，子类看不到父类被new的方法

    * ⭐实方法可以被覆盖（new）


## try catch finally执行顺序

先看代码

```c#
static int Test()
{
    var a = 1;
    try
    {
        return a;
    }
    catch (Exception)
    {

        throw;
    }
    finally
    {
        Console.WriteLine($"finally before:{a}");
        a++;
        Console.WriteLine($"finally end:{a}");
    }
}

Console.WriteLine($"test:{Test()}");

```

结果

```
finally before:1
finally end:2
test:1
```

⭐分析

即使finally中对变量x进行了改变，但是不会影响返回结果

