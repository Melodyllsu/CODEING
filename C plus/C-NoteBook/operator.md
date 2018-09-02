# 操作符重载与临时对象
两种写法：作为成员函数，作为全局函数（像操作符这种必须作为全局函数）  
## 操作符重载带有隐含的this，指向左边对象的   
任何成员函数都有一个隐藏的this指针，指向调用者  
```c
inline complex&
__doapl(complex* ths, const complex& r)
{
ths->re += r.re;
ths->im += r.im;
return *ths;
}
...
inline complex&
complex::operator += (const complex& r)
{
return __doapl (this, r);
}
...
{
complex c1(2,1);
complex c2(5);
c2 += c1;
}
...
inline complex&
complex::operator += ( /*this,这边默认传递了一个c2的指针*/ const complex& r)
{
return __doapl (this, r);
}
```
## 传递着无需知道接收者以什么形式接收   

## 全局函数  
用非成员函数重载操作符，没有this指针，不能返回引用，因为函数的结果是临时对象typename(),没有名字   
```c
complex c1(2,1);
complex c2;
complex();//临时对象
complex(4,5);
```

