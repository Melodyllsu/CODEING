# 侯捷老师《C++面相对象高级开发》（下）   
## 2. 转换函数  
```c
operator double() const{
    return (double)(m_numerator / m_denominator);//分数类中的一个转换函数，将分数转换成double
}
···
//使用
double d = 4+f; //若f是分数类，此时调用的是转换函数   
```
## 3. non-explicit-one-argument ctor  
给构造函数加上explict，拒绝隐式类型转换，防止执行时的二义性  

## 4. pointer-like classes   
智能指针  
```c
template<class T>
class shared_ptr
{
public:
    T& operator*() const
    { return *px; }
    T* operator->() const
    { return px; }
    shared_ptr(T* p) : px(p) { }
private:
    T*  px;
    long* pn;
...
}

```
## 5. function-like classes, 仿函数    
仿函数其实是重载了()的类  

## 6. namespace的经验之谈   
将自己的函数放入命名空间，防止变量名或函数名重复   

## 7. 类模板  
## 8. 函数模板  
类别不用指明，编译器会进行实参推导   
## 9. 成员模板  
在模板中的模板,一般将类中的构造函数写成成员模板，使得类更加泛化       
使用范例： pair<Base1, Base2> p2(pair<Derived1, Derived2>());//用子类初始化父类，调用父类的构造函数，此时父类的构造函数为成员模板的写法   
## 10. 模板特化  
针对模板的一些特殊情况写不同的代码   
泛化部分依然存在，遇到特化的情况先选择特化   
## 11. 模板偏特化  
个数上的偏特化， 比如说有两个模板参数，指定其中一个，绑定的循序要从左往右   
范围上的偏特化， 比如将任意类型缩小为任意类型的指针  
```c
template <typename T>
class C
{
    ...
};
template <typename T>
class C<T*>  //偏特化版本  
{
    ...
};
```
## 12. 模板模板参数  
```c
template<typename T, template <typename T> class Container>
```
## 13. C++标准库  

## 14. ranged-base for （C++11）  
```c
for ( decl : coll)
{
    statement
}
vector<double> vec;
...
for(auto elem : vec){//传值的方式
    cout << elem << endl;
}
for(auto& elem : vec){//传引用，可以改变原数值 
    elem *= 3;
}

```
## 15. 引用  
引用不能重新代表其它的变量  
java中所有的变量都是reference   

## 16. 复合&继承关系下的构造和析构   
子类构造函数执行之前会调用父类的构造函数  
## 17. 虚指针和虚表（vptr&vtbl）  
重要，动态绑定，虚机制  
## 18. this  
## 19. 动态绑定   
A<-B<-C  
```c
B b;
A a = (A)b;
a.vfunc1();//a不是指针，这是一个静态绑定   

A* pa = new B;
pa->vfunc1();//动态绑定，虚函数表

pa = &b;
pa->vfunc1();//动态绑定，虚函数表
```
## New Delete  
底层是用malloc实现的   







