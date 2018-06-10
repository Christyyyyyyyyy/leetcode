# Vector  
Vector are **sequence containers** representing arrays that **can change in size**.  
> 在C++中，Vector是一个十分有用的**序列容器**，简单来说，Vector是一个能够**存放任意类型的动态数组**，数组的大小是可以改变的。
  
Just like arrays, vectors use **contiguous storage locations** for their elements, which means that their elements can also be accessed using **offsets on regular pointers to its elements**, and just as efficiently as in arrays. But unlike arrays, their **size can change dynamically**, with their storage being handled automatically by the container.  
> 元素的储存：contiguous storage location  
> 元素的访问：除了像数组一样通过下标以外，offsets on regular pointers to its elements也可以  
> 数组的大小：是可以动态改变的，container会处理好。

  
  ### Container properties
  1. Sequence  
  Elements in sequence containers are ordered in a strict linear sequence. Individual elements are accessed by their position in this sequence.  
  >> 元素在sequence container里是线性排序的。
  >> 像数组一样，元素可通过其在序列里的顺序被访问到。
  2. Dynamic array  
  Allows direct access to any element in the sequence, even through pointer arithmetics, and provides relatively fast addition/removal of elements at the end of the sequence.  
  >> 在end of the sequence，提供了增删操作。
  >> 且尽管是动态的数组，元素的访问也可以通过pointer arithmetics。
  3. Allocator-aware  
  The container uses an allocator object to dynamically handle its storage needs.  
  >> 容器使用分配器对象（allocator object）来动态处理其储存需求。
  
  ### 使用方法
```
  #include<iostream>
  #include<vector>

  using namespace std;

  int main(){
      vector<int> test;   //建立一个vector，int为元素数组的数据类型，test为动态数组名
      test.push_back(1);
      test.push_back(2);   //把1和2压入vector，因此，test[0]是1， test[1]是2
      cout << test[0] << endl;
      cout << test[1] << endl;
      return 0;
  }
```
>> 输出：  
>> 1  
>> 2

### 基本操作
1. 头文件：  
```
#include<vector>
```
2. 创造vector对象：  
```
vector<int>test;
```
3. 尾部插入数字：
```
test.push_back(a);
```
4. 尾部删除元素：
```
test.pop_back();
```
4. 使用下标访问元素：（记住从下标是从0开始的）
```
cout << test[0] << endl;
```
5. 使用迭代器访问元素：
```
    vector<int>::iterator k;
    for(k = test.begin(); k != test.end(); k++)
        cout << *k << endl;
```
>> 输出：  

>> 1  
>> 2  
>> 3  
>>> 所以k本质上是一个指针。  
要注意，迭代器<>里的数据类型不是一直是int的！应该根据与vector的数据类型一致：  
```
int main(){
    vector<char> test; //建立一个vector，int为元素数组的数据类型，test为动态数组名
    test.push_back('a');
    test.push_back('b');//把a和b压入vector，因此，test[0]是a， test[1]是b
    test.push_back('c');
    vector<char>::iterator k;
    for(k = test.begin(); k != test.end(); k++)
        cout << *k << endl;
    return 0;
 }
```
>> 输出：  

>> a  
>> b  
>> c 
6. 向量大小：
```
test.size();
```
7. 任意位置插入元素：
```
test.insert(test.begin() + i, a);  //在第i+1个元素前插入a；
```
8. 删除任意位置元素：
```
test.erase(test.begin()+2);  //删除第三个元素
```
9. 删除区间：
```
test.erase(test.begin()+i, test.begin()+j);  //删除区间[i,j-1]，注意区间从0开始！！！
```
10. 清空向量：
```
test.clear();
```

### 其他函数
refer to: http://www.cplusplus.com/reference/vector/vector/assign/ 
- vector::assign  
> Assign vector content: assign new contents to the vector, replacing its current contents, and modifying its size accordingly.  
- vector::at  
> Access element: returns a reference to the element at position n in the vector.  
```
for(unsigned i = 0; i < myvector.size(); i++)
  myvector.at(i) = i;
``` 
- vector sort函数
https://www.cnblogs.com/cj695/p/3863142.html  

http://www.cnblogs.com/cj695/p/3863142.html#3795071
