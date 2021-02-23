## 拷贝构造函数  
```
Point pt2 = pt1; //int a = b，拷贝构造函数
```
默认情况下，编译器会自动生成拷贝构造函数  
拷贝构造函数形式：  
```
    Point(const Point &rhs) //特定的参数
    : _ix(rhs._ix)
    , _iy(rhs._iy)
    {
        cout << "拷贝构造函数" << endl;
    }
```
