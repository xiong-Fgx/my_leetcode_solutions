# STL库的笔记

## 1. C++11使用哈希重新实现了原来的一些容器
- 例如unordered_map、unordered_set、unordered_multimap、unordered_multiset，接口不变，但是其中的实现方式均使用哈希实现，从而运行速度更快。