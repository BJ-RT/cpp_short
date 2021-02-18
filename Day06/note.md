## 类外实现成员函数  
见demo文件夹  
成员函数本质上是inline函数  
## 构造函数&初始化表达式   
```
class Point
{
public:
    //构造函数的作用：完成数据成员的初始化
    //构造函数可以重载
    Point(int ix, int iy)
    : _ix(ix)//初始化表达式
    , _iy(iy)
    {
        cout << "构造函数" << endl; 
        //_ix = ix;
        // _iy = iy;
    }
    void print()
    {
        cout << "(" << _ix
             << "," << _iy
             << ")" << endl;
    }
private:
    int _ix;
    int _iy;
};

int main()
{
    Point pt(1, 2); //对象在创建的时候会调研构造函数
                    //构造函数不能通过对象以.的形式调用
    cout << "pt = ";
    pt.print();
 
```

## 对象的销毁  
对指针式字符串的初始化操作，需用申请堆空间的方式，+1是因为\0结尾  
```
 Computer::Computer(const char *brand, float price)
 : _brand(new char[strlen(brand) + 1]())                             
 , _price(price)
 ```
 用析构函数进行空间回收  
 ```
Computer::~Computer()
{
    cout << "析构函数" << endl;
    if(_brand)
    {
        cout << "回收堆空间" << endl;
        delete [] _brand;
        _brand = nullptr;//防止野指针 
    }
```
析构函数可以显式调用，但是不建议，可能发生意外  
内存  
```

Computer gCom("xiaomi", 7000); //全局对象

int main()
{
    gCom.print();
    
    Computer com("lenovo", 5500); //栈对象
    com.print();

    Computer *pc = new Computer("HUAWEI", 12000); //堆对象
    pc->print();
    delete pc; //如果不delete，会导致不执行析构函数
    pc = nullptr;

```











