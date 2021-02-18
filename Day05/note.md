## 函数的类型  
```
//函数的类型
int func(int x)  //函数类型：int (int)
{
    return x;
}

int (*pf)(int); //定义一个int类型的、返回类型为int的函数指针

int main()
{
    pf = func; //函数名就是函数的入口地址
    cout << pf(12) << endl;
    return 0;
}

```
## 函数重载  
C语言不支持函数重载  
```
//C++支持函数重载
//原理：名字改编
//步骤：当函数名字相同的时候，根据参数个数、顺序、类型去改编函数名字
//仅仅返回类型不同不足以成为函数的重载
int add(int x, int y)
{
    return x+y;
}

int add(int x, int y, int z)
{
    return x+y+z;
}

int main()
{
    int a = 3, b = 4, c = 5;
    cout << "add(a,b) = " << add(a,b) << endl;
    cout << "add(a,b,c) = " << add(a,b,c) << endl;
    
    cout << endl;

    //C++兼容C的时候，不支持名字改编，即C不支持名字改编
    //extern "C" {xxxxx}可将C++按照C的方式编译，不进行名字改编
    int *pInt = static_cast<int *>(malloc(sizeof(int)));
    //malloc的返回类型是void *,要把它强制转化成int *
    memset(pInt, 0, sizeof(int));
    free(pInt);
    pInt = nullptr;

    return 0;
```  
## C与C++的混合编程  
```
#ifdef _cplusplus //C++的内置宏
extern "C"
{
#endif
int add(int x, int y)
{
  return x + y;
}
#ifdef _cplusplus
} //end of extern C
```
## 默认参数  
```
int add(int x, int y, int z = 0)//z设置默认参数
{                               //默认参数必须从右往左设置，与入栈出栈有关
    return x + y + z;
}

int main()
{
    int a = 3, b = 4, c = 5;
    cout << "add(a,b) = " << add(a,b) << endl;//因为z有默认参数，所以可以传两个参
    cout << "add(a,b,c) = " << add(a,b,c) << endl;//也可传三个参
```
## bool类型  
不是0，就是1  
非0值都是true  
内存空间大小为1

## inline函数  
带参数的宏定义使用方式与函数类似  
```
#define multiply(x,y) x*y
cout << "multiply(a,b) = " << multiply(a,b) << endl;
```  
宏定义发生在预处理阶段，即字符串的替换  
普通函数的调用是入栈出栈的过程  
内联函数inline兼顾以上特点，不用入栈出栈而是进行字符串的替换，提高运行效率  
见inline文件
对于inline函数而言，不能分成头文件与实现文件（不能将声明与实现分开）  
如果一定要将声明与实现分开，可以使用#include包含实现文件  
inline函数要简单，最好不要加while/for  

## 异常安全  
为了使代码更加健壮  
```
throw 表达式
try-catch语句块
```
## C和C++风格字符串  
C:  
```
    //C语言定义字符串，字符数组
    //C语言是以'\0'结尾的
    char str1[] = "hello";
    char str2[] = "world";

    str1[0] = 'H'; //改变字符串内容
    printf("str1 = %s\n", str1);

    //str1 = nullptr;error字符数组只能改变某个内容，不能改变本身

    //C语言获取字符串长度的方式
    size_t len1 = sizeof(str1);
    size_t len2 = sizeof(str2);

    printf("len1 = %lu\n", len1);
    printf("len2 = %lu\n", len2);

    const char *pstr = "12345"; //指针方式，默认为const
    printf("pstr = %s\n", pstr);
    
    pstr = "hubeiwuhan";
    printf("pstr = %s\n", pstr);
    //pstr[0] = 'H';error指针方式本身可以改变，但内容不能改变，因为是const char*
    cout << sizeof(pstr) << endl; //普通方式两种方式都可计算长度
    cout << strlen(pstr) << endl; //指针方式只能用strlen计算长度

    //C语言拼接字符串
    size_t len = len1 + len2 - 1;
    char *pstr1 = static_cast<char *>(malloc(len));
    memset(pstr1, 0, len);
    strcpy(pstr1, str1); //拷贝
    strcat(pstr1, str2); //拼接

    printf("pstr1 = %s\n", pstr1);

    free(pstr1);
```
C++：  
```
    //1、C风格字符串可以转化为C++风格字符串
    //     C++    C
    string s1 = "hello";
    string s2 = "world";
    
    //拼接字符串
    string s3 = s1 + s2;
    cout << "s1 = " << s1 << endl
         << "s2 = " << s2 << endl
         << "s3 = " << s3 << endl;

    cout << endl;

    //2、C++风格字符串转化为C风格字符串
    const char *pstr = s3.c_str();
    cout << "pstr = " << pstr << endl;
    
    //3、获取C++风格字符串长度
    size_t len1 = s3.size();
    size_t len2 = s3.length();
    cout << "len1 = " << len1 << endl
         << "len2 = " << len2 << endl;

    //4、遍历C++风格字符串
    for(size_t index = 0; index != s3.size(); ++index)
    {
        cout << s3[index] << " ";
    }
    cout << endl;

    //5、C++风格字符串的拼接
    string s4 = s3 + "wuhan";
    cout << "s4 = " << s4 << endl;
    string s5 = s4 + 'A';
    cout << "s5 = " << s5 << endl;

    s5.append(s1); //追加
    cout << "s5 = " << s5 << endl;
```
## 程序内存分配方式  





















