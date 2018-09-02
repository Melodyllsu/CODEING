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
operator.md  

-------------------------------














