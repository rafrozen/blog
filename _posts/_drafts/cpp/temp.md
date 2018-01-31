# temp


## inline

对比于普通函数，内敛函数没有普通函数的参数压栈，生成汇编里面call调用，返回参数，执行return, 普通函数调用时首先立即保存现场->跳转到执行的调用函数->执行->返回保存的地址继续执行。

内敛函数是将函数体嵌入到每一个调用处，实行类似宏定义那样。所以简短的1-5行代码执行效率是非常高的

**宏定义函数　只是替换**
## functional point

```cpp
void (*p)(int,int);
p = func;
typedef bool Func(const string&, const string&);  
typedef decltype(Func) *Funcp; //decltype要自己加上*，它不会自动转换成指针
```
c语言函数指针的定义形式：返回类型 (*函数指针名称)(参数类型,参数类型,参数类型，…);

c++函数指针的定义形式：返回类型 （类名称::*函数成员名称）（参数类型，参数类型，参数类型，….); 

解引用函数指针时可以用*,也可以不用，同样取地址也可以要也可以不用&

```

/* Readable!!! */
typedef int func(int);
typedef func* func_ptr;
typedef func_ptr* func_ptr_ptr;

func_ptr_ptr p;
```

## override
` override` 关键字，可以避免派生类中忘记重写虚函数的错误
  C++11 中的` override` 关键字，可以显式的在派生类中声明，哪些成员函数需要被重写，如果没被重写，则编译器会报错。

1)  公有继承

　　纯虚函数      => 继承的是：接口 (interface)

　　普通虚函数   => 继承的是：接口 + 缺省实现 (default implementation)

　　非虚成员函数 =>继承的是：接口 + 强制实现 (mandatory implementation)

2)  never redefine an inherited non-virtual function

3)  在声明需要重写的函数后，加关键字 override


## final

另一方面，如果你希望函数不要再被派生类进一步重写，你可以把它标识为final。可以在基类或任何派生类中使用final。在派生类中，可以同时使用override和final标识。
## nullprt
引入nullptr的原因，这个要从NULL说起。对于C和C++程序员来说，一定不会对NULL感到陌生。但是C和C++中的NULL却不等价。NULL表示指针不指向任何对象，但是问题在于，NULL不是关键字，而只是一个宏定义(macro)。

## static


定义全局静态变量 ：在全局变量前面加上关键字static，该全局变量变成了全局静态变量。全局静态变量有以下特点：
- 在全局数据区内分配内存
- 如果没有初始化，其默认值为0
- 该变量在本文件内从定义开始到文件结束可见

定义局部静态变量：在局部静态变量前面加上关键字static，该局部变量便成了静态局部变量。静态局部变量有以下特点:
- 该变量在全局数据区（静态存储区包括全局变量和static变量）分配内存
- 如果不显示初始化，那么将被隐式初始化为0
- 它始终驻留在全局数据区，直到程序运行结束
- 其作用域为局部作用域，当定义它的函数或语句块结束时，其作用域随之结束。

 定义静态函数：在函数的返回类型加上static关键字，函数即被定义成静态函数。静态函数有以下特点：

- 静态函数只能在本源文件中使用

- 在文件作用域中声明的inline函数默认为static


> 在C++语言中新增了两种作用：定义静态数据成员或静态函数成员

 定义静态数据成员。静态数据成员有如下特点：

 内存分配：在程序的全局数据区分配

 初始化和定义：静态数据成员定义时要分配空间，所以不能在类声明中初始化（但是static const 可以在类的定义中初始化）

静态成员函数。静态成员函数与类相联系，不与类的对象相联系。静态成员函数不能访问非静态数据成员。
- 静态成员函数可以访问静态成员变量和和静态成员函数
- 非静态成员函数也可以访问静态成员变量和和静态成员函数
- 静态成员函数没有this指针，无法访问属于类对象的非静态成员变量和非静态成员函数
- 由于没有this指针的额外开销，因此静态成员函数与类的非静态成员函数相比速度上会有少许的增长
- 静态成员函数/变量属于整个类，没有this指针，该类的所有对象共享这些静态成员函数/变量 包括该类派生类的对象。即派生类对象与基类对象共享基类的静态数据成员
- 非静态成员函数/变量属于类的具体的对象，this是缺省的
- 静态成员变量在类内声明，且必须带static关键字；在类外初始化，且不能带static关键字
- 静态成员函数在类内声明，且必须带static关键字；在类外实现，且不能带static关键字


1. 重写 (override):

      父类与子类之间的多态性。子类重新定义父类中有相同名称和参数的虚函数。

1) 被重写的函数不能是 static 的。必须是 virtual 的 ( 即函数在最原始的基类中被声明为 virtual ) 。

2) 重写函数必须有相同的类型，名称和参数列表 (即相同的函数原型)

3) 重写函数的访问修饰符可以不同。尽管 virtual 是 private 的，派生类中重写改写为 public,protected 也是可以的

2. 重载 (overload):

      指函数名相同，但是它的参数表列个数或顺序，类型不同。但是不能靠返回类型来判断。
      


3. 重定义 (redefining):

      子类重新定义父类中有相同名称的非虚函数 ( 参数列表可以不同 ) 。

    `static` same as member function
    
## 虚函数

虚函数的第一个成本：必须为每个拥有虚函数的类耗费一个 vtbl 空间，其大小视虚函数的个数（包括继承而来的）而定。不过，一个类只会有一个 vtbl 空间，所以一般占用空间不是很大。

不要将虚函数声明为 inline ，因为虚函数是运行时绑定的，而 inline 是编译时展开的，即使你对虚函数使用 inline ，编译器也通常会忽略。

虚函数的第二个成本：必须为每个拥有虚函数的类的对象，付出一个指针的代价，即 vptr ，它是一个隐藏的 data member，用来指向所属类的 vtbl。

调用一个虚函数的成本，基本上和通过一个函数指针调用函数相同，虚函数本身并不构成性能上的瓶颈。

虚函数的第三个成本：事实上等于放弃了 inline。（如果虚函数是通过对象被调用，倒是可以 inline，不过一般都是通过对象的指针或引用调用的）

### NULL在C中的定义
在C中，习惯将NULL定义为void*指针值0：
```
#define NULL (void*)0  
```
### NULL在C++中的定义
在C++中，NULL却被明确定义为整常数0

根本原因和C++的重载函数有关。C++通过搜索匹配参数的机制，试图找到最佳匹配（best-match）的函数，而如果继续支持void*的隐式类型转换，则会带来语义二义性（syntax ambiguous）的问题。
```

void foo(int i);  
void foo(char* p)  
  
foo(NULL); // which is called?  
```

如果我们的编译器是支持nullptr的话，那么我们应该直接使用nullptr来替代NULL的宏定义。正常使用过程中他们是完全等价的。
(-lstdc++=c++0x)

## 模板编写
如果要分开编写
`.hh` 写声明, `.cc`写定义,则调用此模板的cc文件应该加
```cpp
#include "xxx.cc"
```
而非
```cpp
#include "xxx.hh"
```

隐式调用也就是隐式的参数类型推导，根据参数类型决定函数模板的编译，如：

```
    // implicitly
    c_max(1, 2);

    // explicitly
    c_max<double>(1, 2);
```

那么这么做有什么用呢？当使用两个不同类型的参数调用时，会有什么结果呢？

```
   c_max(1, 2.1);
```
编译时会出现类似下面的错误：
> error: no matching function for call to ‘c_max(int, double)’

这个时候就需要使用显示的调用，如

```
   c_max<double>(1, 2.1);
```

## argv[]   argc
`argc`是命令行总的参数个数，`argv[]`是`argc`个参数，其中第0个参数`argv[0]`是程序的全名，以后的参数`argv[1],argv[2]...`