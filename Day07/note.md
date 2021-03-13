# 拷贝构造函数  
```
Point pt2 = pt1; //int a = b，拷贝构造函数
```
默认情况下，编译器会自动生成拷贝构造函数  
拷贝构造函数形式:  
### 默认形式 浅拷贝：拷贝前后都指向同一块内存空间  
```
    Point(const Point &rhs) //特定的参数
    : _ix(rhs._ix)
    , _iy(rhs._iy)
    {
        cout << "拷贝构造函数" << endl;
    }
```  
### 自定义形式 深拷贝：拷贝指向新的内存空间，改变被拷贝函数，拷贝函数不变  
```
Computer::Computer(const Computer &rhs)
: _brand(new char[strlen(rhs._brand) + 1]())
, _price(rhs._price)
{
    cout << "拷贝构造函数" << endl;
    strcpy(_brand, rhs._brand);
```
### 拷贝构造函数的调用时机：  
1、当用一个已存在的对象初始化另一个新对象时  
2、当实参和形参都是对象，进行实参与形参的结合时  
```
void func(Point pt) //形参 Point pt = pt2
{
    pt.print();
}

void test2()
{
    Point pt2(3,4);
    cout << "pt2 = ";
    pt2.print();
    func(pt2); //实参 实参与形参结合时调用拷贝构造函数
}
```
3、当函数的返回值是对象，函数调用完成返回时  
```
int func0() //类比
{
    int number = 10;
    return number;
}
Point func3()
{
    Point pt3(12,34);
    return pt3; //当函数的返回值是对象，函数调用完成返回时调用拷贝构造函数
                //但是编译器默认不调用
                //通过g++ Point1.cc -fno-elife-constructors（编译器的优化选项）可显示调用
}
```  
### 拷贝构造函数的引用符号是否可以去掉？  
不可以，如果去掉，在进行拷贝构造函数调用的时候，符合拷贝构造函数的调用时机2，如此，就会循环调用拷贝构造函数，直到栈溢出  
### 拷贝构造函数的const是否可以去掉？  
不可以，非const左值引用不能绑定到右值，当传递到右值的时候，在执行拷贝构造函数的时候，就无法匹配右值，就会报错  
### 左值与右值的区别：  
可以取地址的是左值，反之右值，临时对象属于右值，字面值常量属于右值，右值一般存在寄存器或者暂时存在栈上  

# this指针  
```
    //实质：指向对象本身
    //隐藏于（非静态的）成员函数的第一个参数的位置
    void print(/* Point * const this */)
    {
        //this = nullptr; error,this指针是const,不能改变指向
        //this->_ix = 10000; //只认this
        cout << "(" << _ix // this->_ix 
             << "," << _iy // this->_iy 
             << ")" << endl;
    }
```
# 赋值运算符函数  
```
    cout << endl << "将pt2 = pt1" << endl;
    //pt2 = pt1;
    pt2.operator=(pt1); //赋值运算符函数，编译器自动生成
```
### 到目前为止编译器自动生成的函数：  
1、无参的构造函数  
2、析构函数  
3、拷贝构造函数  
4、赋值运算符函数  
### 默认形式（类比浅拷贝）：
```
    Point &operator=(const Point &rhs)
    {
        cout << "赋值运算符函数" << endl;
        _ix = rhs._ix;
        _iy = rhs._iy;
        return *this; //返回对象本身(this)
    }
```
### 自定义形式（类比深拷贝）：  
```
Computer &Computer::operator=(const Computer &rhs)
{
    cout << "赋值运算符函数" << endl;
    if(this != &rhs) //防止自己对自己赋值时发生错误
    {
      delete [] _brand; //释放原来指向的空间
      _brand = nullptr;
      _brand = new char[strlen(rhs._brand) + 1](); //申请新空间
      strcpy(_brand, rhs._brand);
      _price = rhs._price;
    }
    return *this;
}
```
### 赋值运算符函数的返回引用符号可以去掉吗？  
不能，否则在执行return语句的时候，会符合拷贝构造函数的调用时机3，又会进行一次拷贝操作  
### 赋值运算符函数的返回值类型可以不是类类型(Point/Computer)吗?  
不能，  
# 特殊数据成员的初始化  
4种特殊函数：构造函数、析构函数、拷贝构造函数、赋值运算符函数  
4种特殊数据成员：常量成员、引用成员、类对象成员、静态成员  
## 常量数据成员
```
private:
    const int _ix; //常量数据成员（常量在定义的时候必须进行初始化）
    const int _iy;
```
```
    Point(int ix = 0, int iy = 0)
    : _ix(ix) //对常量数据成员进行初始化
    , _iy(iy)

```
```
    //const版本的成员函数与非const版本的成员函数可以同时存在（可以重载）
    //被const修饰的成员函数表示该函数不会修改类内部的数据成员，作用是为了保护数据不被意外修改，提高代码强健性。
    void print();
    void print() const; //建议先定义const版本的
```
```
Computer com1("lenovo", 5500); //非const版本的调用非const成员函数
com1.print();

const Computer com2("apple", 9500); //const版本的调用const成员函数
com2.print();
                                //非const版本的可以调用const版本的成员函数，反之不行
```
## 引用数据成员  
```
    int &_ref; //引用数据成员必须绑定实体，在内存中占一个指针大小的空间（8字节）
```
```
    , _ref(_ix) //引用数据成员绑定实体
```
空类占用一个字节，来区分不同对象  
## 类对象成员  
```
class Line
{
public:
    Line(int x1, int y1, int x2, int y2)
    : _pt1(x1, y1) //类对象成员初始化
    , _pt2(x2, y2)
    {}
```
```
private:
    Point _pt1; //类对象成员必须进行初始化
    Point _pt2;
};
```
## 静态数据成员  
静态数据成员不受private、protect等的限制，不占内存，可在类外访问，被该类的所有对象共享  
```
private:
    char *_brand;
    float _price;
    static float _totalPrice; //必须进行初始化
```
```
Computer::Computer(const char *brand, float price)
: _brand(new char[strlen(brand) + 1]())
, _price(price)
, _totalPrice(0) //静态数据成员不能在初始化列表初始化
```
```
using std::endl;
float Computer::_totalPrice = 0.0; //在类外初始化（多文件时在实现文件.cc中初始化）
Computer::Computer(const char *brand, float price)
```
静态成员函数的定义：  
```
    static void printTotalPrice(); //声明
```
```
void Computer::printTotalPrice() //实现的时候不加static
{

}
```
静态成员函数无隐含的this指针，因为静态成员是全局的东西，被全体成员共享    
```
void Computer::printTotalPrice()
{
    _price = 100; //error, 静态成员函数不能访问非静态的数据成员，反之可以
    print(); //error, 也不能访问非静态的成员函数，反之可以
}
```
静态成员函数的调用：  
```
void Computer::printTotalPrice()
{
    cout << "静态成员函数 = " << _totalPrice << endl;
}
```
```
    Computer::printTotalPrice(); //静态成员函数可以使用类名加作用域的方式调用
```














 
 
 
 
 
 
 
