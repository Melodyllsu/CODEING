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
