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













