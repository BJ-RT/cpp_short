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
拷贝构造函数的调用时机：  
1、当用一个已存在的对象初始化另一个新对象时  
2、当实参和形参都是对象，进行实参与形参的结合时  
3、当函数的返回值是对象，函数调用完成返回i时  

