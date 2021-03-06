## 拷贝构造函数  
```
Point pt2 = pt1; //int a = b，拷贝构造函数
```
默认情况下，编译器会自动生成拷贝构造函数  
拷贝构造函数形式:  
浅拷贝：拷贝前后都指向同一块内存空间  
```
    Point(const Point &rhs) //特定的参数
    : _ix(rhs._ix)
    , _iy(rhs._iy)
    {
        cout << "拷贝构造函数" << endl;
    }
```  
深拷贝：拷贝指向新的内存空间，改变被拷贝函数，拷贝函数不变  
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
可以取地址的是左值，反之右值，右值一般存在寄存器或者暂时存在栈上  




