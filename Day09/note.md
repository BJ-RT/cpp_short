# 文件IO
## vector动态数组   
用法：  
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
```cpp

```









