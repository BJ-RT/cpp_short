# 多基继承及相关问题
...  
# 基类与派生类之间的转化
...  
# 派生类对象间的拷贝控制
```cpp
    Base base(11.11);
    base.show();

    cout << endl;
    Derived derived(22.22, 33.33);
    derived.print();

    cout << endl << "派生类向基类进行转换 " << endl;
    //向上转型都是安全
    base = derived;//1、派生类对象可以赋值给基类
    /* base.operator=(derived);//Base &operator=(const Base &rhs) ===> const Base &rhs = derived; */
    base.show();

    cout << endl;
    Base &ref = derived;//2、基类的引用可以绑定到派生类的对象
    ref.show();

    cout << endl;
    Base *pbase1 = &derived;//3、基类的指针可以指向派生类的对象
    pbase1->show();

    cout << endl << "基类向派生类进行转换 " << endl;
    Base base1(11.11);
    Derived derived1(22.22, 33.33);
#if 0
    derived1 = base1;//1、error，不能从基类的对象转化为派生类对象
    derived1.operator=(base1);//Derived &operator=(const Derived &rhs) === > const Derived &rhs = base1

    cout << endl;
    Derived &ref2 = base1;//2、error，不能将派生类的引用绑定到基类的对象
#endif
#if 1
    cout << endl;
    //不安全的向下转型
    Derived *pderived1 = static_cast<Derived *>(&base1);//3、error，不能将派生类的指针指向基类的对象

    Base *pbase2 = &derived1;
    Derived *pderived2 = static_cast<Derived *>(pbase2);//安全的向下转型

#endif
```
# 派生类对象间的拷贝控制
详见pdf  
DerivedCopyControl.cc  
# 关联式容器set与map
associativeContainer.cc  
## set
set的特点：                                                                
1、关键字必须唯一，不能出现关键字相同的元素   
2、默认情况下，按照关键字进行升序排列   
3、底层实现是红黑树   
associativeContainer.cc
实现形式1：  
```cpp
    set<int> number = {1, 2, 5, 9, 7, 4, 5, 6, 7, 2, 1};
    //遍历
    for(auto &c : number)
    {
        cout << c << "  ";
    }
    cout << endl;
```
```cpp
    int arr[10] = {1, 6, 9, 5, 6, 3, 7, 2, 7};
    set<int> number(arr, arr + 10);
    //遍历2
    //迭代器：抽象的认为是一个指针
    set<int>::iterator it;
    for(it = number.begin(); it != number.end(); ++it)
    {
        cout << *it << "  ";
    }
```
元素的查找：  
```cpp
   size_t cnt1 = number.count(10); 
   size_t cnt2 = number.count(5); 
   cout << "cnt1 = " << cnt1 << endl
        << "cnt2 = " << cnt2 << endl;

   cout << endl;
   /* set<int>::iterator it2 = number.find(7); */
   auto it2 = number.find(17);
   if(it2 == number.end())
   {
       cout << "该元素不存在set中" << endl;
   }
   else
   {
       cout << "该元素存在set中 : " << *it2 << endl;
   }
```
插入：  
```cpp
   auto ret = number.insert(7);

    int arr[5] = {1, 10, 79, 4, 8};
    number.insert(arr, arr + 5);
```
set不支持下标访问  
set不支持修改  
## map
map就是高级的set  
pair:  
```cpp
    std::pair<int, string> number = {123, "wuhan"};
    cout << number.first << "   " << number.second << endl;
```
map:  
```cpp






























