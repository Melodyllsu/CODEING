# 侯捷老师《C++面相对象高级开发》   
听课笔记  

## 1. C++编程简介  
- 内存为C++基础，不涉及C++11    
- C++ 包含 C++语言以及C++标准库  
- 推荐书籍   
《C++ Primer》、 《Effective C++》、 《STL源码剖析》  
-----------------
## 2. 头文件与类声明  
- 防御式声明   
```c
 #ifndef __COMPLEX__
 #define __COMPLEX__
 ...
 #endif
```  
- 头文件的布局  
0.前置声明  
1.类-声明  
2.类-定义  
- 模板类    
---------------------
## 3. 构造函数  
- 函数若在class body内定义完成，便自动成为inline候选人，最后结果还是由编译器决定  
- 访问级别  
private：一般变量是类自己使用的   
public：一般函数是被外界调用的   
- 构造函数  
```c
//构造函数的独特初始化方法 
complex (double r = 0, double i = 0)//默认参数  
  :re (r), im(i)  //初始化，规范， 效率高  
{ }               //赋值
```
- 构造函数的重载  
如果构造函数有默认值，要注意重载时注意不能冲突。  
如以下两个函数就是冲突的：  
```c
complex (double r = 0, double i = 0)
  :re (r), im(i)  
  { }
complex ():re (0), im (0)
  { }

```
------------------
## 4. 参数传递与返回值  
- 构造函数被放在private：不允许被外界调用    
设计模式：Singleton 单例模式  
```c
class A{
public: 
  static A& getInstance();
  setup() {...}
private：
  A()；
  A(const A& rhs);
  ...
};
A& A::getInstance ()
{
  static A a;
  return a;
}
//那么如何获得对象呢？  
A::getInstance().setup();

```
- 在函数的后面加const  
不会改变数据的函数一定要随手加上const   

- 参数传递 pass by value vs. pass by reference (to const)  
为了效率，尽量所有的参数传递都传引用  
- 返回值的传递  
为了效率也尽量引用传递（可以的情况下）   
- friend  
 直接拿到私有成员变量   
 在类中声明为friend  
- 相同class的各个objects互为friends  
 
- **class设计时的注意点总结**   
1. 数据一定放在private：  
2. 参数尽可能用引用来传，需不需要const看情况  
3. 返回值也尽量用引用来传（可以的情况下）
4. 函数的body后面应该加const时一定要加  
5. 构造函数设计时尽量用它独特的初始化  
---------------------------
## 5. 操作符重载与临时对象  
- operator.md  

-------------------------------
## 6. 复习Complex类的实现过程  
1. 写防卫式的声明  
2. 写出类的头  
3. 思考复数需要哪些数据，是什么类型，放在private中  
4. 考虑构造函数，考虑是否需要默认值，考虑参数的传递形式。写出构造函数的初值列，设初值
5. 设计成员函数  
6. 需要访问私有成员的函数，注意函数是否需要加const  
7. 写函数定义  
-------------------
## 7. 三大函数：拷贝构造，拷贝赋值，析构  
- cope.md   
浅拷贝&深拷贝  

-----------
## 8. 堆、栈与内存管理  
**stack,是存在于某作用域的一块内存空间。**  
- 自动对象  
出了作用域就自动调用析构函数。  
- 静态对象  
在作用域外依然存在，程序结束后才调用析构函数。
- 全局对象  
全局存在， 程序结束后才调用析构函数。  
**heap，是指由操作系统提供的一块globle内存空间。要手动delete。**
- new出来的对象  
要手动delete。否则内存泄漏。  
```c
Complex* pc = new Complex(1,2);
//new的作用：  
void* mem = operator new( sizeof(Complex) ); //分配內存
pc = static_cast<Complex*>(mem); //转型  
pc->Complex::Complex(1,2);//构造函数
```
new 先分配内存，再调用构造函数   
delete 先调用析构函数（对象的主体），再释放内存（对象的指针）  

- 动态分配获得的内存块   
在VC下一定是16的倍数   
内存块的两头有两个叫做cookie的头，4x2个字节。4个字节记录了内存块的长度。最后一位记录了这块内存的状态（获得内存标志位是1，归还内存标志位是0）。  
如：00000031，获得一块大小为3x16的内存   

----------------
## 9. 复习String类的实现过程   

## 10. 扩展补充：类模板，函数模板，及其他   
- static  
静态数据和静态函数，只有一份，没有this pointer。  
静态函数只能处理静态数据。  
静态数据要在类外面进行定义，用于分配内存，无所谓是否赋初值。  
```c
class Account {
public:
static double m_rate;
static void set_rate(const double& x) { m_rate = x; }
};
double Account::m_rate = 8.0; 
```
调用静态函数有两种方式：1.通过对象调用；2.通过类名调用   
- 类模板  
```c
template<typename T>
class complex
{
public:
complex (T r = 0, T i = 0)
: re (r), im (i)
{ }
complex& operator += (const complex&);
T real () const { return re; }
T imag () const { return im; }
private:
T re, im;
friend complex& __doapl (complex*, const complex&);
};
```
- 函数模板  
```c
//标准库中的算法都是函数模板类型的
template <class T>
inline
const T& min(const T& a, const T& b)
{
return b < a ? b : a;//<的重载在T类中完成，分工明确   
}
```
- namespace  

--------------------------
## 11. 组合与继承  
类与类之间的关系： 继承、复合、委托   
- 复合Composition，（has-a）:一个类中有另一个东西   
同步创建  
设计模式：Adapter  
```c
template <class T, class Sequence = deque<T> >
class queue {
...
protected:
Sequence c; // 底層容器，复合
public:
// 以下完全利用 c 的操作函數完成
bool empty() const { return c.empty(); }
size_type size() const { return c.size(); }
reference front() { return c.front(); }
reference back() { return c.back(); }
// deque 是兩端可進出，queue 是末端進前端出（先進先出）
void push(const value_type& x) { c.push_back(x); }
void pop() { c.pop_front(); }
};
```
构造由内而外  
Container的构造函数首先调用Component的default构造函数，然后才执行自己  

析构由外而内  
Container的析构函数首先执行自己，然后才调用Component的析构函数  
- 委托Delegation，Handle-Body（pimpl），Composition by reference，有个指针指向另一个东西   
不同步创建   

- 继承Inheritance， 表示is-a  
构造由内而外  
析构由外而内  
父类的析构函数必须是virtual否者会出现undefined behaviour    
-------------
## 12. 虚函数与多态    
 - non-virtual函数：不希望子类重新定义它   
 - virtual函数：希望子类重新定义它，已经有默认定义了   
 - pure virtual函数：希望子类一定要重新定义它，对它没有默认定义  
虚函数的用法：  
设计模式：Template Method----先写好框架，将无法定义的方法留给子类去实现   

## 13. 委托相关设计  
设计模式：Composite----委托与继承结合
设计模式：Prototype  
让未来的子类自己创建一份蓝本，传给父类，并在父类需要的时候让父类复制  








