# 写时复制原理讲解
...  
# 写时复制技术实现
...  
# 作业讲解之Log4cpp讲解与string重载
...  
# 面向对象之继承
父类（基类）、子类（派生类）  
```cpp
class Derived : public/protected/private Base //继承方式
{
//修饰方式
public:
protected:
private:
}
```
派生类的生成过程包含三个步骤：  
1、吸收基类成员  
2、改造基类成员  
3、添加自己新的成员  
```cpp
class Point3D
: public Point
{
```
不能继承的：  
构造函数、析构函数、用户重载的operator new/delete运算符、用户重载的operator=运算符、友元关系  
## 派生类对象的构造
派生类构造函数的一般格式为：  
```cpp
派生类名（总参数表）：基类构造函数（参数表）
{
    //函数体
};
```
### 1、基类无显式构造函数，派生类有： 
```cpp
class Base
{
public:
private:
    double _base;
};

class Derived
: public Base
{
public:
    Derived(double derived)
    : Base()
    , _derived(derived)
    {
        cout << "Derived(double)" << endl;
    }
private:
```
### 2、派生类无显示构造函数，基类有
```cpp

```
### 3、
### 4、
详见PDF  













































