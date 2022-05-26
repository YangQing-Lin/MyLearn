#### STL

--------------

##### vector 

变长数组，数组长度动态变化，倍增的思想

##### string 

字符串，substr()：返回某一个子串，c_str()：返回 string 对应的字符数组的头指针

##### queue 

队列，push()：往队尾插入元素，front()：返回队头元素，pop()：把队头弹出

##### priority_queue

优先队列（堆），push()：往堆里插入一个元素，top()：返回堆顶，pop()：把堆顶弹出

##### stack

栈，push()：往栈顶添加元素，top()：返回栈顶元素，pop()：弹出栈顶元素

##### deque

双端队列，队头队尾都可以插入删除，支持随机访问，加强版 vector

##### set，map，multiset，multimap

基于平衡二叉树（红黑树，平衡二叉树的一种），动态维护有序序列

##### unordered_set，unordered_map，unordered_multiset，unordered_multimap

基于哈希表

##### bitset

压位

------------

##### 所有容器都支持的函数：

```c++
// 时间复杂度是 O(1) 的
a.size(); // 返回容器里面的元素个数
a.empty(); // 返回容器是不是空的，如果是空的就返回 true，如果不是空的就返回 false
```



