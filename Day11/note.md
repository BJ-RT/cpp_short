# 一、友元  
建议少用，违背封装性，详见视频  
# 二、运算符重载
本质还是函数重载  
形式：  
```cpp
返回类型 operator 运算符(参数表)
{
    //...
}
```
不能重载的运算符：`sizeof`等  
重载的规则：详见PDF  
## 第一种形式：
```cpp
    double getReal() const
    {
        return _dreal;
    }

    double getImag() const
    {
        return _dimag;
    }

//1、函数重载之普通函数的形式(自由函数、全局函数),需要get函数作为支撑
Complex operator+(const Complex &lhs, const Complex &rhs)
{
    cout << "Complex operator+(const Complex &, const Complex &)" << endl;
    return Complex(lhs.getReal() + rhs.getReal()
                   , lhs.getImag() + rhs.getImag());
}
```
## 第二种形式：  
```cpp
    //2、运算符重载之成员函数的形式,形式上与加号违背
    Complex operator+(const Complex &rhs)
    {
        cout << "Complex operator+(const Complex &)" << endl;
        return Complex(_dreal + rhs._dreal, _dimag + rhs._dimag);
    }
```
## 第三种形式：
```cpp
class Complex
{
    friend Complex operator+(const Complex &lhs, const Complex &rhs);
public:

//3、函数重载之友元函数,加号推荐使用友元形式，简洁
Complex operator+(const Complex &lhs, const Complex &rhs)
{
    cout << "friend Complex operator+(const Complex &, const Complex &)" << endl;
    return Complex(lhs._dreal + rhs._dreal
                   , lhs._dimag + rhs._dimag);
}
```
为什么有的要用&operator?  
https://blog.csdn.net/weixin_43515966/article/details/93746576  
即返回* this的要加&  
## 复合赋值运算符
```cpp
    //建议以成员函数的形式进行重载,改变对象本身，建议以成员函数进行重载
    Complex &operator+=(const Complex &rhs)
    {
        cout << "Complex &operator+=(const Complex &)" << endl;
        _dreal += rhs._dreal;
        _dimag += rhs._dimag;

        return *this;
    }
```
## 自增自减运算符
```cpp
a = 7;
(++a) = 8, a = 8;
(a++) = 7, a = 8;
```
```cpp
    //前置++比后置++执行效率高，后置++有临时对象tmp产生，多调用一次拷贝构造函数
    Complex &operator++()
    {
        cout << "Complex &operator++()" << endl;
        ++_dreal;
        ++_dimag;

        return *this;
    }

    Complex operator++(int)//注意：为了与前置++不一样，所以参数列表中
                            //加上int用来与前置++进行区分，没有参数传递的意思
    {
        cout << "Complex operator++(int)" << endl;
        Complex tmp(*this);
        ++_dreal;
        ++_dimag;

        return tmp;
    }
```
## 输入流运算符的重载
即用cout << c1取代c1.print()  
```cpp
    //对于输出流运算符而言，左操作数必须是流对象，而如果重载
    //的输出流运算符作为成员函数，那第一个参数就是this指针，
    //这个就与运算符重载的规则违背,所以输出流运算符不能以成员
    //函数的形式重载
    friend std::ostream &operator<<(std::ostream &os, const Complex &rhs);
    friend std::istream &operator>>(std::istream &is, Complex &rhs);
private:
```
```cpp
std::ostream &operator<<(std::ostream &os, const Complex &rhs)
{
    if(0 == rhs._dreal && 0 == rhs._dimag)
    {
        os << 0 << endl;
    }
    else if(0 == rhs._dreal)
    {
        os << rhs._dimag << "i" << endl;
    }
    else
    {
        os << rhs._dreal;
        if(rhs._dimag > 0)
        {
            os << " + " << rhs._dimag << "i" << endl;
        }
        else if(rhs._dimag < 0)
        {
            os << " - " << (-1) * rhs._dimag << "i" << endl;
        }
        else
        {
            os << endl;
        }

    }
    return os;
}
```
```cpp
void readDouble(std::istream &is, double &number)
{
    while(is >> number, !is.eof())
    {
        if(is.bad())
        {
            std::cerr << "The istream is bad" << endl;
            return;
        }
        else if(is.fail())
        {
            is.clear();//重置流的状态
            is.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            cout << "pls input a double data " << endl;
        }
        else
        {
            cout << "number = " << number << endl;
            break;
        }

    }
}
std::istream &operator>>(std::istream &is, Complex &rhs)
{
    /* is >> rhs._dreal >> rhs._dimag; */
    readDouble(is, rhs._dreal);
    readDouble(is, rhs._dimag);
    return is;
}
```
## 函数调用运算符的重载
...  
## 下表访问运算符重载
...  
## 箭头解引用运算符的重载
...





























