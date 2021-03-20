# 一、实现一个自定义的string类 
String.cc  
```cpp
#include <iostream>
#include <string.h>

using std::cout;
using std::endl;

class String
{
public:
    String()
    : _pstr(nullptr)
    {
        cout << "无参构造函数" << endl;
    }

    String(const char *pstr)
    : _pstr(new char[strlen(pstr) + 1]())
    {
        cout << "C风格字符串" << endl;
        strcpy(_pstr, pstr);
    }

    String(const String &rhs)
    : _pstr(new char[strlen(rhs._pstr) + 1]())
    {
        cout << "拷贝构造函数" << endl;
        strcpy(_pstr, rhs._pstr);
    }

    String &operator=(const String &rhs)
    {
        cout << "赋值运算符函数" << endl;
        if(this != &rhs)
        {
            delete [] _pstr;
            _pstr = nullptr;

            _pstr = new char[strlen(rhs._pstr) + 1]();
            strcpy(_pstr, rhs._pstr);
        }

        return *this;
    }

    void print() const
    {
        if(_pstr)
        {
            cout << "pstr = " << _pstr << endl;
        }
    }

    ~String()
    {
        cout << "析构函数" << endl;
        if(_pstr)
        {
            delete [] _pstr;
            _pstr = nullptr;
        }
    }

private:
    char *_pstr;
};

int main()
{
    String str1;
    str1.print();

    cout << endl;

    String str2 = "Hello,world";
    String str3("hhhhhh");
    str2.print();
    str3.print();

    cout << endl;

    String str4 = str2;
    str4.print();

    cout << endl;

    str3 = str2;
    str3.print();

    return 0;
}
```
# 二、编写一个类，实现简单的栈
栈中有以下操作：  
元素入栈void push(int);  
元素出栈void pop();  
读出栈顶元素int top();  
判断栈空bool emty();  
判读栈满bool full();  
如果栈溢出，程序终止。  
栈的数据成员由存放10个整型数据的数组构成。先后做如下操作：  
创建栈  
将10入栈  
将12入栈  
将14入栈  
读出并输出栈顶元素  
出栈  
读出并输出栈顶元素  
stack.cc  
```cpp
#include <iostream>

using std::cout;
using std::endl;

class Stack
{
public:
    Stack(int size)
    : _size(size)
    , _top(-1)
    , _data(new int[_size]())
    {
        cout << "构造函数" << endl;
    }

    bool empty()
    {
        return (-1 == _top);
    }

    bool full()
    {
        return (_top == _size - 1);
    }

    //元素入栈
    void push(const int &value)
    {
        if(!full())
        {
            _data[++_top] = value;
        }
        else
        {
            cout << "栈满" << endl;
            return;
        }
    }

    void pop()
    {
        if(!empty())
        {
            --_top;
        }
        else
        {
            cout << "栈空" << endl;
            return;
        }
    }

    //获取栈顶元素
    int top()
    {
        return _data[_top];
    }

    ~Stack()
    {
        cout << "析构函数" << endl;
        if(_data)
        {
            delete [] _data;
            _data = nullptr;
        }
    }

private:
    int _size;
    int _top;
    int *_data;
};

int main()
{
    Stack st(10);
    cout << "此时栈是否为空？" << st.empty() << endl;
    st.push(1);
    cout << "此时栈是否为满？" << st.full() << endl;
    for(size_t idx = 2; idx != 12; ++idx)
    {
        st.push(idx);
    }
    cout << "此时栈是否为满？" << st.full() << endl;

    //元素出栈
    while(!st.empty())
    {
        cout << st.top() << endl;
        st.pop();
    }
    cout << "此时栈是否为空？" << st.empty() << endl;
    return 0;
}
```
# 三、编写一个类，实现简单的队列  
队列中有以下操作：  
元素入队void push()  
元素出队void pop()  
读取队头元素int front()  
读取队尾元素int back()  
判断队列是否为空bool empty()  
判断队列是否已满bool full()  
注意循环队列的使用  
```cpp
#include <iostream>

using std::cout;
using std::endl;

class Queue
{
public:
    Queue(int sz = 10)
    : _size(sz)
    , _front(0)
    , _rear(0)
    ,_data(new int[_size]())
    {
        cout << "构造函数" << endl;
    }

    bool empty()
    {
        return _front == _rear;
    }

    bool full()
    {
        return _front == (_rear + 1) % _size;
    }

    void push(int value)
    {
        if(!full())
        {
            _data[_rear++] = value;
            _rear %= _size;
        }
        else
        {
            cout << "队列已满" << endl;
            return;
        }
    }

    void pop()
    {
        if(!empty())
        {
            ++_front;
            _front %= _size;
        }
        else
        {
            cout << "队列为空" << endl;
            return;
        }
    }

    int front() const
    {
        return _data[_front];
    }

    int back() const
    {
        return _data[(_rear - 1 + _size) % _size];
    }

    int getFront() const
    {
        return _front;
    }

    int getRear() const
    {
        return _rear;
    }

    ~Queue()
    {
        cout << "析构函数" << endl;
        if(_data)
        {
            delete [] _data;
            _data = nullptr;
        }
    }

private:
    int _size;
    int _front;
    int _rear;
    int *_data;
};

int main()
{
    Queue que;
    cout << "此时队列是否为空？" << que.empty() << endl;
    que.push(1);
    cout << "此时队列是否为满？" << que.full() << endl;
    for(size_t idx = 2; idx != 12; ++idx)
    {
        que.push(idx);
    }
    cout << "此时队列是否为满？" << que.full() << endl;

    cout << "打印队列头尾" << endl << endl;
    cout << "队头元素" << que.front() << endl;
    cout << "队头指针" << que.getFront() << endl;
    cout << "队尾元素" << que.back() << endl;
    cout << "队尾指针" << que.getRear() << endl;

    while(!que.empty())
    {
        cout << que.front() << endl;
        que.pop();
    }
    cout << "此时队列是否为空？" << que.empty() << endl;
    return 0;
}
```
# 四、用C++实现一个双向链表  
```cpp
struct Node
{
    int data;
    Node * pre;
    Node * next;
};

//逆置一个单链表
//逆置双向链表

class List
{
 public:
    List();
    ~List();
    void push_front(int data);//在头部进行插入
    void push_back(int data);//在尾部进行插入
    void pop_front();//在链表头部进行删除
    void pop_back();//在链表尾部进行删除
    bool find(int data);//在链表中进行查找
    void insert(int pos, int data);//在制定位置后面插入pos
    void display() const;//打印链表
    void erase(int data);//删除一个指定的节点
 private:
    Node * _head;
    N0de * _tail;
    int _size;
);
```
详见视频  
# 五、统计文章中的单词和词频  
Dictionary.cc  
# 六、日志系统  
由于服务器没有交互界面，所以要用日志系统来记录动作    
分为业务日志与系统日志  
# 七、Log4cpp  
github搜索：fffaraz/awesome-cpp -----> logging -----> Log4cpp  
见思维导图和log4cpp安装文件和本节PDF  
testLog4cpp.cc  

















