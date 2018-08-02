### enum(枚举)
```c
enum spectrum{red, orange, yellow, green, blue, violet, indigo, ultraviolet };
//1. 该语句让spectrum成为新类型的名称，spectrum被称为枚举
//2. 将red等作为符号常量，它们将对应整数值0~7，这些常量叫作枚举量。
```
1. 默认情况下，将整数值赋给枚举量，第一个枚举量的值为0，第二个为1，以此类推。可通过显示指定整数值来覆盖默认值。  
2. 在不进行强制类型转换的情况下，只能将定义枚举时使用的枚举量赋给这种枚举变量。也就是说spectrum变量受到限制，只可能有8个值，如果试图将一个非法的值赋给它，则会出错。 
```c
spectrum band;
band = blue;//有效
band = 2000;//无效
```

3. 对于枚举，只定义了赋值运算符，没有定义算数运算符。  
++band;//无效  
4. 枚举量是整型，可以被提升为int型，但int型不能自动转换成枚举型。如果要这么做，可以通过强制类型转换。   

```c
int colour = blue;//有效
band = 3;//无效
band = spectrum(3);//有效，强制类型转换
```

5. 可以使用赋值运算符来显示地设置枚举量的值，指定的值必须是整数。也可以创建多个值相同的枚举量。  
```c
enum bits{first, second = 100, third};//第一个值first在默认情况下为0.后面没有被初始化的枚举量的值将比其前面的大1.third的值为101.
```

6. 枚举的取值范围
通过强制类型转换，可以将取值范围中的任何整数赋给枚举变量，即使这个值不是枚举值。  
确定取值范围，首先要确定上限。找到枚举量的最大值，找到大于这个最大值的最小2次幂，将它减1. 如：最大值若为101，则上限为128-1=127.   
然后确定下限，若最小值不小于0，则取值的下限为0.若最小值小于零，寻找下限方式与找上限方式相同，但加上负号。如：最小值为-6，则下限为-8+1 =-7.  
  
### explicit关键字  
> https://blog.csdn.net/fengbingchun/article/details/51168728    
1. 中的关键字explicit主要是用来修饰类的构造函数，被修饰的构造函数的类，不能发生相应的隐式类型转换，只能以显示的方式进行类型转换。类构造函数默认情况下声明为隐式的即implicit。  
> 转换即是可以由单个实参来调用的构造函数定义了一个从形参类型到该类类型的隐式转换。编译器在试图编译某一条语句时，如果某一函数的参数类型不匹配，编译器就会尝试进行隐式转换，如果隐式转换后能正确编译，编译器就会继续执行编译过程，否则报错。  
2. licit关键字只能用于类内部的构造函数声明上，而不能用在类外部的函数定义(函数实现)上，它的作用是不能进行隐式转换；explicit关键字作用于单个参数的构造函数，如果构造函数有多个参数，但是从第二个参数开始，如果各参数均有默认赋值，也可以应用explicit关键字。
3. 构造函数只有一个参数时，会进行自动隐式转换，当构造函数参数个数超过或等于两个时自动取消隐式转换，当只有一个必须输入的参数，其余的为有默认值的参数时使用explicit也起作用。  
4. 一般只将有单个参数的构造函数声明为explicit，而拷贝构造函数不要声明为explicit。  
5. licit关键字只能对用户自己定义的对象起作用，不对默认构造函数起作用。此关键只能够修饰构造函数。无参数的构造函数和多参数的构造函数总是显示调用，这种情况在构造函数前加explicit无意义。
6. 当不希望进行自动类型转换时用explicit，标准库的许多构造函数都是explicit的。  
- explicit.hpp：
```c
#ifndef FBC_MESSY_TEST_EXPLICIT_HPP
#define FBC_MESSY_TEST_EXPLICIT_HPP
 
// reference Bjarne Stroustrup sample
class String{
public:
	explicit String(int n) {};
	String(const char *p) {};
};
 
void test_explicit();
 
#endif // FBC_MESSY_TEST_EXPLICIT_HPP

```
- explicit.cpp：
``` c
#include <explicit.hpp>
 
static void test1()
{
	String s1 = 'a'; // 错误：不能做隐式char->String转换
	String s2(10); // 可以：调用explicit String(int n);
	String s3 = String(10); // 可以：调用explicit String(int n);再调用默认的复制构造函数
	String s4 = "Brian"; // 可以：隐式转换调用String(const char *p);再调用默认的复制构造函数
	String s5("Fawlty"); // 可以：正常调用String(const char *p);
}
 
static void f(String)
{
 
}
 
static String g()
{
	f(10); // 错误：不能做隐式int->String转换
	f("Arthur"); // 可以：隐式转换，等价于f(String("Arthur"));
	return 10; // 同上
}
 
void test_explicit()
{
	test1();
	g();
}
```


