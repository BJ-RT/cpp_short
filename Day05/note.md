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























