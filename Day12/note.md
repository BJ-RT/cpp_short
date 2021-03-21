# 类型转换(冷门）
其他类型与自定义类型之间的转换  
## 其他类型->自定义类型 
简单，隐式转换  
## 自动义类型->其他类型
Point2.cc  
调用相应的构造函数  
调用类型转换函数  
```cpp
operator 目标类型()
{
    //...
}
```
# 类作用域
```cpp
int number = 1;

namespace wd
{
int number = 20;

class Test
{
public:
    Test(int num)
    : number(num)
    {

    }
    void print(int number)
    {
        cout << "形参number = " << number << endl;
        cout << "数据成员number = " << this->number << endl;
        cout << "数据成员number = " << Test::number << endl;
        cout << "wd命名空间中的number = " << wd::number << endl;
        cout << "全局number = " << ::number << endl;
    }
private:
```
# 内部类
Point4.cc  
# 单例模式的自动释放
测试内存泄漏：valgrind  






















