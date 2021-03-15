# 一、delete原理  
浅拷贝容易造成double free问题  
# 二、对象的组织  
## 1、const对象  
```cpp
const Point pt(1, 2);
pt.print();
```
## 2、指向对象的指针  
```cpp
类名 * 指针名 [=初始化表达式]；

Point pt(1, 2);
Point * p1 = nullptr;
Point * p2 = &pt;
Point * p3 = new Point(3, 4);
Point * p4 = new Point[5];

p3->print();
(*p3).print();
```
## 3、对象数组  
```cpp
类名 数组名[对象个数]；

Point pts[2] = {Point(1, 2), Point(3, 4)};
Point pts[] = {Point(1, 2), Point(3, 4)};
Point pts[5] = {Point(1, 2), Point(3, 4)};
或
Point pts[2] = {{1, 2}, {3, 4}}; 换成大括号
Point pts[] = {{1, 2}, {3, 4}};
Point pts[5] = {{1, 2}, {3, 4}};

pts[0].print();
```
## 4、堆对象  
```cpp
Point * pt1 = new Point(11, 12);
pt1->print();
delete pt1;
pt1 = nullptr;

Point * pts = new Point[5]();
pts[0] = {1, 2};
pts[1] = Point(3, 4);
pts->print();
(pts + 1)->print();
delete [] pts;
```
# 三、单例模式  
一个类只创建一个对象  
```cpp
#include <iostream>

using std::cout;
using std::endl;

//设计要求：一个类只能创建一个对象
class Singleton
{
public:
    static Singleton *getInstance() //类中创建静态堆对象
    {
        if(nullptr == _pInstance)
        {
            _pInstance = new Singleton();
        }

        return _pInstance;
    }
    
    static void destroy()
    {
        if(_pInstance)
        {
            delete _pInstance;
            _pInstance = nullptr;
        }
    }

private:
    Singleton()
    {
        cout << "构造函数" << endl;
    }
    
    ~Singleton()
    {
        cout << "析构函数" << endl;
    }

    static Singleton *_pInstance;
};

Singleton * Singleton::_pInstance = nullptr;

//Singleton gS1;
//Singleton gS2; error, 全局对象不可

int main()
{
    //Singleton s1;
    //Singleton s2; error, 栈对象不可
    
    //Singleton *ps1 = new Singleton();
    //Singleton *ps2 = new Singleton(); error, 类外的堆对象不可
    
    Singleton *ps1 = Singleton::getInstance();
    Singleton *ps2 = Singleton::getInstance(); 
    cout << "ps1 = " << ps1 << endl;
    cout << "ps2 = " << ps2 << endl;
    Singleton::destroy();
    return 0;
}
```
# 四、new与delete表达式  
## new表达式工作步骤：  
1、调用名为operator new的标准库函数，分配足够大的原始的未类型化的内存，以保存指定类型的一个对象  
2、运行该类型的一个构造函数初始化对象  
3、返回指向新分配并构造的构造函数对象的指针  
## delete表达式工作步骤：  
1、调用析构函数，回收对象中数据成员所申请的资源  
2、调用名为operator delete的标准库函数释放该对象所用的内存  
Student.cc:  
```cpp
#include <iostream>
#include <string.h>
#include <stdlib.h>

using std::cout;
using std::endl;

class Student
{
public:
    Student(int id, const char *name)
    : _id(id)
    , _name(new char[strlen(name) + 1]())
    {
        cout << "构造函数" << endl;
        strcpy(_name, name);
    }
    
    void print() const
    {
        cout << "id = " << _id << endl
             << "name = " << _name << endl;
    }

    void *operator new(size_t sz) //静态成员函数，无this指针
    {
        cout << "void *operator new(size_t)" << endl;
        void *pret = malloc(sz);
        return pret;
    }

    void operator delete(void *pret) //静态成员函数，无this指针 
    {
        cout << "void operator delete(void *)" << endl;
        free(pret);
    }

    ~Student()
    {
        cout << "析构函数" << endl;
        if(_name)
        {
            delete [] _name;
            _name = nullptr;
        }
    }

private:
      int _id;
      char *_name;
};

int main()
{
    Student *pstu = new Student(123, "xiaohong");
    pstu->print();
    //对象的销毁与析构函数的执行等价吗？
    //不等价，对于堆对象而言，析构函数的执行只是对象销毁的一个步骤，还要执行delete操作
    delete pstu;
    return 0;
}
```
## 要求一个类只能创建栈对象  
StudentStack.cc  
```cpp
          Student stu(345, "xiaoming");
       ✗  Student *pstu = new Student(123, "xiaohong");
```
方法：将operator new设置为私有  
```cpp
private:
    void *operator new(size_t sz) //静态成员函数，无this指针
    {
        cout << "void *operator new(size_t)" << endl;
        void *pret = malloc(sz);
        return pret;
    }
```
栈对象创建的条件是什么？  
构造函数与析构函数都必须是public  
## 要求一个类只能创建堆对象  
StudentHeap.cc  
```cpp
      ✗   Student stu(345, "xiaoming");                                        
          Student *pstu = new Student(123, "xiaohong");
           
      ✗  delete pstu; //但不能执行delete操作了
```
方法：将析构函数设置为私有，并添加destory函数解决不能delete问题  
```cpp
    void destroy()
    {
        delete this; //this指针指向对象本身
    }
private:
    ~Student()
    {
        cout << "析构函数" << endl;
        if(_name)
        {
            delete [] _name;
            _name = nullptr;
        }
    }
```
delete表达式工作步骤举例：   
执行析构函数从而释放“xiaoming"  
执行delete从而释放pstu指向的堆对象  
# 五、C++输入输出流  
### 详见pdf  
输入：文件 -> 内存    
输出：内存 -> 文件  
## C++常用流类型：  
标准I/O：键盘显示器  
文件I/O：磁盘文件  
串I/O：内存中指定空间，通常用字符数组作为存储空间   
### 流的状态：  
```cpp
badbit  bad()
failbit fail()
eofbit  eof()
goodbit good()
```
## 标准I/O
StandrdIO.cc：  
```cpp
#include <iostream>
#include <string>
#include <limits>

using std::cout;
using std::endl;
using std::cin;
using std::string;
using std::cerr;

void printStreamStatus()
{
    cout << "cin.badbit = " << cin.bad() << endl
         << "cin.failbit = " << cin.fail() << endl
         << "cin.eofbit = " << cin.eof() << endl
         << "cin.goodbit = " << cin.good() << endl;
}

void test()
{
    int number = 0;
    printStreamStatus();
    cin >> number;
    printStreamStatus();
    cin.clear(); //重置流的状态
    //cin.ignore(1024, '\n'); //清空缓冲区
    cout << endl;
    printStreamStatus();
    cout << "number = " << number << endl;
    
    string line;
    cin >> line;
    cout << "line = " << line << endl;
}

void test2()
{
    int number = 0;
    cout << "Please input number: " << endl;
    while(cin >> number, !cin.eof()) //逗号表达式
    {
        if(cin.bad())
        {
            cerr << "The stream is bad" << endl;            
            return; //退出当前函数test2
        }
        else if(cin.fail())
        {
            cin.clear();
            cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            cout << "Please input valid int number: " << endl;
        }
        else
        {
            cout << "number = " << number << endl;
        }
        //ctrl + d : 正常退出
    }
}

int main()
{
    test2();
    return 0;
}
```
### 缓冲区  
buffer.cc:  
```cpp
    //cout << "123" << endl; //刷新缓冲区，并且换行
    //cout << "123" << flush; //刷新缓冲区，但不换行
    cout << "123" << ends; //不能换行与刷新缓冲区
    sleep(5);
```
隐式转换：  
```cpp
Point pt = 5; //这样赋值会发生隐式转换，调拥一次拷贝构造函数
```
## 文件I/O
FileIO01.cc  
```cpp
void test()
{
    //默认情况下，对于文件输入流而言，当文件不存在的时候，就报错
    ifstream ifs("Point1.cc");
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }
        
    string line;
    while(ifs >> line) //while(ifs >> line, !ifs.eof())
    {                  //对于文件输入流而言，默认以空格为分隔符
        cout << line << endl;
    }

    ifs.close();
}

void test2()
{
    ifstream ifs("Point1.cc");
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }
        
    string line;
    while(getline(ifs, line)) //按行读取 
    {                  
        cout << line << endl;
    }

    ifs.close();
}
```

