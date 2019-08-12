# STL库的笔记

## 1. C++11使用哈希重新实现了原来的一些容器
- 例如unordered_map、unordered_set、unordered_multimap、unordered_multiset，接口不变，但是其中的实现方式均使用哈希实现，从而运行速度更快。

## 2. stream的三种情况
1. 基于控制台的IO
    ```
    #include<iostream>
    ```
2. 基于文件的IO
    ```
    #include<fstream>
    ```
3. 基于字符串的IO
    ```
    #include<sstream>
    
    //下面是一个用于读取两个'/'之间的字符信息。

    stringstream ss(path);
    while(getline(ss,tmp,'/')) {
            if (tmp == "" or tmp == ".") continue;
            if (tmp == ".." and !stk.empty()) stk.pop_back();
            else if (tmp != "..") stk.push_back(tmp);
        }
    ```