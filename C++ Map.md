# C++ Map 
Maps are **associative containers** that store elements formed by a combination of a **key value** and a **mapped value**, following a specific order. 
> Map是**关联容器**，里面放的是**key value** - **mapped value** 这样子的一个组合形式的元素，并且这些元素是按照一定的顺序排列的。**有序的！**
  
In a map, the **key values** are generally used to **sort** and uniquely **identify** the elements, while the **mapped values** store the **content** associated to this key. The **types** of key and mapped value may **differ**, and are grouped together in member type value_type, which is a **pair type combining both**
```
typedef pair<const Key, T> value_type;
```
> key value: 用于 sort排序， 以及 identify the elements  
> mapped value：放的是 与 key 对应的 content内容。 
> key 与 mapped value的类型可以不同  

Internally, the **elements** in a map are always **sorted by its key** following a specific strict weak ordering criterion indicated by its **internal comparison object** (of type Compare).  
> map里的元素因为是 key-value 的对，所以排序的话，是拿元素的key来然后对元素进行排序的，怎么排是由map的internal comparison object决定的。

map containers are generally slower than unordered_map containers to access individual elements by their key, but they allow the direct iteration on subsets based on their order.  
> map是有序的，所以总的来说，要通过key值访问到某个元素的时候，比unordered_map会慢

The mapped values in a map can be accessed directly by their corresponding key using the bracket operator ((operator[]).  
>访问某个元素的时候，通过 mapname[key] 可以返回 mapped values

Maps are typically implemented as binary search trees.  
>Map的实现： binary search tree

  
  ### Container properties
  1. Associative 
  Elements in associative containers are referenced by their **key** and not by their **absolute position** in the container.  
  > 你要访问 associative container里的元素，不能说像数组一样，通过下标或者绝对位置访问到，必须是key值。
  2. Ordered  
  The elements in the container follow a strict order at all times. All inserted elements are given a position in this order.
  > Map里的元素一定是有序的！！！有序的！！！有序的！！！
  3. Map  
  > Each element associates a key to a mapped value: Keys are meant to identify the elements whose main content is the mapped value.  
  4. Unique keys  
  > No two elements in the container can have equivalent keys.  
  5. Allocator-aware  
  > The container uses an allocator object to dynamically handle its storage need
  

### 基本操作
1. 初始化／声明：  
```
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<char,int> first;

    first['a'] = 10; //The mapped values can be accessed directly by the corresponding key using operator[]. 
    first['b'] = 30;
    first['c'] = 50;
    first['d'] = 70;

    map<char,int> second(first.begin(),first.end()); // 把已有的map的一段区间作为参数，初始化得到一个新的map

    map<char,int> third(second); // 把已有的map作为参数，初始化得到一个新的map

    return 0;
}
```
2. =：  
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> first;
    map<char,int> second;

    first['x'] = 8;
    first['y'] = 16;
    first['z'] = 32;

    second = first; // second now contains 3 ints
    first = map<char,int>(); // and first in now empty

    cout << "Size of first: " << first.size() << endl;
    cout << "Size of second:" << second.size() << endl;
}
```
> 1. = 用于赋mapped value的值by mapname[key] = 。。。
> 2. 两个同类型的map之间可以通过 = 直接copy，例如上面的 second = first；
> 3. 要把一个已有的map清空，可以通过 = map<>(); 
3. Capacity相关函数：
> 1. empty函数:test whether container is empty
> 2. size函数: Return container size
> 3. max_size函数: Return maximum size.   
Returns the maximum number of elements that the map container can hold.  
This is the maximum potential size the container can reach due to known system or library implementation limitations, but the container is by no means guaranteed to be able to reach that size: it can still fail to allocate storage at any point before that size is reached.
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<int,int>mymap;
    mymap[1] = 11;
    mymap[2] = 22;
    mymap[3] = 33;

    cout << mymap.size() << endl;
}
```
>> 输出：
>> 3
4. Element access相关：
> 1. operator[]  
> If k matches the key of an element in the container, the function returns a reference to its mapped value.  
> If k does not match the key of any element in the container, the function **inserts** a new element with that key and returns a reference to its mapped value. Notice that this always **increases the container size by one**, even if no mapped value is assigned to the element( the element is construced using its default constructor).   
> A similar member function, map::at, has the same behavior when an element with the key exists, but **throws an exception when it does not.**
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,string>mymap;

    mymap['a'] = "an element";
    mymap['b'] = "another element";
    mymap['c'] = mymap['b']; // key=c的元素是新创造的，所以通过[]既可以access到，也可以创造有新的key的元素

    cout << "mymap['a'] is " << mymap['a'] << endl;
    cout << "mymap['b'] is " << mymap['b'] << endl;
    cout << "mymap['c'] is " << mymap['c'] << endl;

    cout << "mymap now contains " << mymap.size() << " elements." << endl;

    return 0;
}
```
>> 输出：
>> mymap['a'] is an element
>> mymap['b'] is another element
>> mymap['c'] is another element
>> mymap now contains 3 elements.
> 2. at 
> Returns a reference to the mapped value of the element identified with key k.  
> If k does not match the key of any element in the container, the function throws an out_of_range exception.
```
#include <map>
#include <string>
#include <iostream>
using namespace std;

int main(){
    map<string,int> mymap = {
            {"alpha",0},
            {"beta",0},
            {"gamma",0}
    };

    mymap.at("alpha") = 10;
    mymap.at("beta") = 20;
    mymap.at("gamma") = 30;

    for( auto& x: mymap){
        cout << x.first << ": " << x.second << endl;
    }

    return 0;
}
```
> 上面的代码还提供了一种遍历map的方法！
>> 输出：
>> alpha: 10  
>> beta: 20  
>> gamma: 30

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
