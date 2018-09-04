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

