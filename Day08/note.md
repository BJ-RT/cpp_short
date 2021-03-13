# 一、delete原理  
浅拷贝容易造成double free问题  
# 二、对象的组织  
## 1、const对象  
```
const Point pt(1, 2);
pt.print();
```
## 2、指向对象的指针  
```
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
```
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
```
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









