# 文件IO
## 输入流
### vector动态数组   
用法：  
FileIO1.cc:  
```cpp
void test3()
{
    string filename("Point3.cc"); //C++风格
    ifstream ifs(filename);
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }
        
    string line;
    vector<string> vec;
    while(getline(ifs, line)) //按行读取 
    {
        vec.push_back(line);
    }

    for(size_t idx = 0; idx != vec.size(); ++idx)
    {
        cout << vec[idx] << endl;
    }

    ifs.close();
}
```
原理：  
vector.cc  
```cpp
void printVectorCapacity(const vector<int> &vec)
{
    cout << "vec.size = " << vec.size() << endl
         << "vec.capacity = " << vec.capacity() << endl;
}

void test()
{
    //vector的扩容机制：当size() == capacity()的时候，如果还要插入元素，那么
    //就会按照2 * capacity()的大小进行扩容，接着将老的空间的元素拷贝到新空间
    //然后将新的元素插入到新的空间后面，最后将老空间进行回收
    vector<int> vec;
    //vec.reserve(20); //预先设置vector空间，超过空间后才进行扩容
    printVectorCapacity(vec);

    cout << endl;

    vec.push_back(1);
    printVectorCapacity(vec);
    
    cout << endl;


    vec.push_back(2);
    printVectorCapacity(vec);

    cout << endl;

    vec.push_back(3);
    printVectorCapacity(vec);

    cout << endl;

    vec.push_back(4);        //当size与capacity相等时发生扩容
    printVectorCapacity(vec);//扩容以2倍扩大
    
    cout << endl;

    vec.push_back(5);
    printVectorCapacity(vec);

    cout << endl;

    for(auto &elem : vec) //遍历
    {
        cout << elem << "  ";
    }

    cout << endl;
}
```
## 输出流
FileIO2.cc  
```cpp
{
    ifstream ifs("Point3.cc");
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }
    
    //对于文件输出流，当文件不存在就创建文件
    //当文件存在的时候，先清空文件，然后再进行写操作
    ofstream ofs("wuhan.txt");
    if(!ofs.good())
    {
        cerr << "ofstream is bad" << endl;
        ifs.close();
        return;
    }

    string line;
    while(getline(ifs, line)) //按行读取 
    {                  
        ofs <<  line << endl;
    }

    ifs.close();
    ofs.close();
}
```
## 输入输出流  
FileIO2.cc  
```cpp
    //对于文件输入输出流而言，当文件不存在的时候就报错
    fstream fs("wd.txt"); //用touch wd.txt创建文件
    if(!fs.good())
    {
        cerr << "fstream is bad" << endl;
        return;
    }
    int number = 0;
    for(size_t idx = 0; idx != 5; ++idx)
    {
        cin >> number;
        fs << number << "  ";
    }

    //出现文件指针一直在尾部的问题
    cout << "pos = " << fs.tellg() << endl; //打印文件指针位置
    fs.seekg(0); //重新置0
    cout << "pos = " << fs.tellg() << endl;

    for(size_t idx = 0; idx != 5; ++idx)
    {
        fs >> number;
        cout << "fs.fail = " << fs.fail() << endl
             << "fs.eof = " << fs.eof() << endl;
        cout << number << "  ";
    }
    cout << endl;
    fs.close();
```
### 文件模式
FileIO3.cc  
读的追加： 
```cpp
void test()
{
    //ifstream ifs("Point3.cc", std::ios::in | std::ios::ate); //尾部追加
    ifstream ifs("Point3.cc", std::ios::in); //
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }
    cout << "pos = " << ifs.tellg() << endl;

    string line;
    while(ifs >> line) 
    {                  
        cout << line << endl;
    }

    ifs.close();
}
```
写的追加：  
```cpp
void test2()
{
    ifstream ifs("Point3.cc");
    if(!ifs.good())
    {
        cerr << "ifstream is not good" << endl;
        return;
    }

    ofstream ofs("wuhan.txt", std::ios::out | std::ios::app);
    if(!ofs.good())
    {
        cerr << "ofstream is not good" << endl;
        ifs.close();
        return;
    }
    
    cout << "pos = " << ofs.tellp() << endl;
    ofs << "hello world" << endl;
    cout << "pos = " << ofs.tellp() << endl;

    ifs.close();
    ofs.close();
}
```
## 串IO  
tip: 委托构造函数    
### 字符串输出流：  
int型数据转化成string型数据：  
StringStream.cc  
```cpp
#include <iostream>
#include <sstream>
#include <string>

using std::cout;
using std::endl;
using std::ostringstream;
using std::string;

string IntToString(int number)
{
    ostringstream oss;
    oss << number;
    string str1 = oss.str();
    return str1;
}

void test()
{
    string str1 = IntToString(10);
    cout << "str1 = " << str1 << endl;
}

int main()
{
    test();
    return 0;
}
```
### 字符串输入输出流：  
StringStream.cc  
```cpp
void test2()
{
    int number1 = 10;
    int number2 = 20;
    stringstream ss;
    ss << "number1= " << number1 << " ,number2= " << number2 << endl;
    string str1 = ss.str();
    cout << str1;

    string key;
    int value;
    while(ss >> key >> value)
    {
        cout << key << "  " << value << endl;
    }
}

void readConfig(const string &filename)
{
    ifstream ifs(filename);
    if(!ifs.good())
    {
        std::cerr << "ifstream is bad" << endl;
        return;
    }

    string line;
    while(getline(ifs, line))
    {
        string key;
        string value;
        istringstream iss(line);
        iss >> key >> value;
        cout << key << "    " << value << endl;
    }

    ifs.close();
}

void test3()
{
    readConfig("my.conf");
}
```
