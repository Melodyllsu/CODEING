## 7. 三大函数：拷贝构造，拷贝赋值，析构    
**以String为例**  
编译器有一套默认的拷贝构造和拷贝赋值函数。只要类中带指针，一定要自己写， 不能用默认的那一套。  
- 构造函数&析构函数     
```c 
String (const char* cstr = 0);//构造函数
String (const String& str);//拷贝构造
String& operator=(const String&  str);//拷贝赋值
~String();
```
```c
inline
String::String(const chat* cstr = 0)
{
  if (cstr) {
    m_data = new char[strlen(cstr)+1];
    strcpy(m_data, cstr);
  }
  else{
    m_data = new char[1];
    *m_data = '\0';
  }
}
inline
String::~String()
{
  delete[] m_data;  //若类中要动态分配就必须析构
}
```
```c
String s1(); 
String s2("hello");

String* p = new String("hello");
delete p;
```
- 拷贝构造      
浅拷贝：只拷贝对象的指针，指向同一块内存。（编译器自带）     
深拷贝：使用拷贝构造函数，创造足够的空间把所有的内容拷贝过去。（自己重写）  
```c
inline
String::String(const String& str)
{
  m_data = new char[ strlen(str.m_data) + 1 ];
  strcpy(m_data, str.m_data);
}
```
```c
String s2(s1);//调用拷贝构造函数  
```
- 拷贝赋值   
```c
inline 
Srting& String::operator=(const String& str)
{
  if(this == &str)//检测自我赋值，防止它把自己先删掉  
    return *this;
    
  delete[] m_data;
  m_data = new char[ strlen(str.m_data) + 1];
  strcpy(m_data, str.m_data);
  return *this;
}
```
```c
s1 = s2;
```
