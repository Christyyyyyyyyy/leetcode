# C++ Set  
Sets are containers that store **unique** elements following a specific order. 
> Set是容器，但是集合里的元素是unique的这样子的一个组合形式的元素，并且这些元素是按照一定的顺序排列的。
  
In a set, **the value of an element** also **identifies it** (the value is itself the key, of type T), and each value must be unique. The value of the elements in a set cannot be modified once in the container (the elements are always const), but they can be inserted or removed from the container.
> 在集合里，是 element的value itself identifies it！，所以 each value must be unique！
> 元素一旦放在集合里以后，就不能被modified anymore，所以 the elements are always const
> 但是元素可以被插入集合里，或者从集合里被移走  


Internally, the elements in a set are always sorted following a specific strict weak ordering criterion indicated by its internal comparison object (of type Compare). 
> 集合里元素的顺序是由它的internal comparison object (of type Compare)决定的

set containers are generally slower than unordered_set containers to access individual elements by their key, but they allow the direct iteration on subsets based on their order. 
> set是有序的，所以总的来说，要通过key值访问到某个元素的时候，比unordered_set会慢（key？？？）

The mapped values in a map can be accessed directly by their corresponding key using the bracket operator ((operator[]).  
>访问某个元素的时候，通过 mapname[key] 可以返回 mapped values

Sets are typically implemented as binary search trees.  
> Set的实现： binary search tree
  
  ### Container properties
  1. Associative 
  Elements in associative containers are referenced by their **key** and not by their **absolute position** in the container.
  > 你要访问 associative container里的元素，不能说像数组一样，通过下标或者绝对位置访问到，必须是key值。
  2. Ordered  
  The elements in the container follow a strict order at all times. All inserted elements are given a position in this order.
  > Set里的元素一定是有序的！！！有序的！！！有序的！！！
  3. Set  
  The value of an element is also the key used to identify it.    
  > 所以Set里元素的key值就是the value of the element！！！
  4. Unique keys  
  No two elements in the container can have equivalent keys.  
  > 因为set里的元素的key值是元素本身，所以unique keys就是 集合里元素不重复 的意思
  5. Allocator-aware  
  > The container uses an allocator object to dynamically handle its storage need



### 基本操作
1. Constructing Sets：  
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> first; // empty set of ints

    int myints[] = {10,20,30,40,50};
    set<int> second(myints,myints+4); //range

    set<int> third(second); // a copy of second

    set<int> fourth(second.begin(), second.end()); // iterator ctor.

    set<int>::iterator it;
    for(it = second.begin(); it != second.end(); it++){
        cout << *it << endl;
    }
    return 0;
}
```  
输出：  
10  
20    
30  
40  
  
  
2. operator =：  
Assigns new contents to the container, replacing its current content.
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    int myints[] = { 12, 82, 37, 64, 15 };
    set<int>first (myints, myints+5);   //set with 5 ints
    // 所以上面提供了一种，从把数组元素提取到集合里的方法

    set<int>second; //empty set

    second = first; // now second contains the 5 ints
    first = set<int>(); // and first is empty

    cout << "Size of first: " << int(first.size()) << endl;
    cout << "Size of second: " << second.size() << endl;
    
    return 0;
}
```
> 1. 上面提供了一种从数组元素中创造集合的做法：set<int>first(myints,myints+5);
> 2. 两个同类型的set之间可以通过 = 直接copy，例如上面的 second = first；
> 3. 要把一个已有的set清空，可以通过 = set<int>(); 注意尖括号里要写上set的类型
3. Capacity相关函数：
> 1. empty函数:test whether container is empty
> 2. size函数: Return container size
> 3. max_size函数: Return maximum size.   
>> 1. empty函数：Test whether container is empty. Returns whether the set container is empty(i.e., whether its size is 0). This function dose not modify the container in any way!  
  
  ```
  #include <set>
  #include <iostream>
  using namespace std;

  int main(){
     set<int> myset;

      myset.insert(20);
      myset.insert(30);
      myset.insert(10);

      cout << "myset contains: ";
      while(!myset.empty()){
         cout << *myset.begin() << ' ' ;
         myset.erase(myset.begin());
     }

      return 0;
  }
  ```
>> 输出：
>> myset contains: 10 20 30  
>> 所以上面用了 while(!myset.empty()){} 来实现循环，然后{}里 通过 *myset.begin() 来访问集合里的第一个元素，然后通过 myset.erase(myset.begin())来踢掉原有的第一个元素。 
  >>2. size函数：

  ```
  #include <set>
  #include <iostream>
  using namespace std;

  int main(){
      set<int> myints;
      cout << "0. size: " << myints.size() << endl;

     for(int i = 0; i < 10; ++i)
          myints.insert(i);
      cout << "1. size: " << myints. size() << endl;

      myints.insert(100);
      cout << "2. size: " << myints.size() << endl;

      myints.erase(5);
      cout << "3. size: " << myints.size() << endl;

      return 0;
  }

  ```  
  输出：  
  0. size: 0  
  1. size: 10    
  2. size: 11    
  3. size: 10  
  >> 3. max_size:  
  Returns the maximum number of elements that the set container can hold.  
  This is the maximum potential size the contaner can reach due to known system or library implementation limitations, but the container is by no means guaranteed to be able to reach that size: it can still fail to allocate storage at any point before that size is reached.  
  
4. Modifiers相关函数：
> insert: Insert elements  
> erase: Erase elements  
> swap: Swap content  
> clear: Clear content  
> 1. **insert**: Extends the container by inserting new **elements**, effectively increasing the container **size** by the number of elements inserted.  
> Because elements in a set are unique, the insertion operation checks whether each inserted element is equivalent to an element already in the container, and if so, the element is not inserted, returning an iterator to this existing element (if the function returns a value).
> Internally, set containers keep all their elements sorted following the criterion specified by its comparison object. **The elements are always inserted in its repective position following this ordering.**  
>> 1) insert single element  
>> 2) iterator insert  
>> 3) range insert
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;
    set<int>::iterator it;

    for(int i = 1; i <= 5; i++){ // insert single elements
        myset.insert(i*10); //set: 10 20 30 40 50
    }
    int myints[] = {5, 10, 15};
    myset.insert(myints, myints+3); // insert with range

    it = myset.begin();
    myset.insert(it,11); // insert with iterator

    for(it = myset.begin(); it != myset.end(); it++){
        cout << *it << " ";
    }
    return 0;
}
```  
> 输出：   
> 5 10 11 15 20 30 40 50     
> 2. **erase**: Removes from the set container either a single element or a range of elements([first,last])  
> This effectively reduces the container size by the number of elements removed, which are destroyed.  
>> 1) erase by iterator  
>> 2) erase by element  
>> 3) erase by range
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;
    set<int>::iterator it;

    //insert some values:
    for(int i = 1; i < 10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90

    it = myset.begin();
    ++it;

    myset.erase(it); // erase by iterator
    myset.erase(40); // erase by elements

    it = myset.find(60);
    myset.erase(it, myset.end()); //erase by range

    cout << "myset contains: " ;
    for(it = myset.begin(); it != myset.end(); ++it)
        cout << ' ' << *it;

    return 0;
}
```  
> 输出：  
> myset contains:  10 30 50  
> erase by iterator的话，要注意iterator是可以进行算术运算的  
> erase by range的话，和insert by range里用了数组（名）不同，这里range是利用iterator来辅助实现的。

> 3. **swap**: Exchanges the content of the container by the content of x, which is another set of the same type. Sizes may differ.  
> After the call to this member function, the elements in this container are those which were in x before the call, and the elements of x are those which were in this. All iterators, references and pointers remain valid for the swapped objects.
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    int myints[] = { 12, 75, 10, 32, 20, 25 };
    set<int> first(myints,myints+3); // 10,12,75
    set<int> second(myints+3,myints+5); // 20,32 !注意下边是从0开始，而且是左闭右开区间

    first.swap(second);

    cout << "first contians: ";
    for(set<int>::iterator it = first.begin(); it != first.end(); it++)
        cout << ' ' << *it;
    cout << endl;

    cout << "second contains: ";
    for(set<int>::iterator it = second.begin(); it != second.end(); it++)
        cout << ' ' << *it;
    
    return 0;
}
```  
> 输出：
> first contians:  20 32
> second contains:  10 12 75  

> 4. **clear**: Removes all elements from the container (which are destroyed), leaving the container with a size of 0.

```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;

    myset.insert(100);
    myset.insert(200);
    myset.insert(300);

    cout << "myset contains: ";
    for(set<int>::iterator it = myset.begin(); it != myset.end(); it++)
        cout << ' ' << *it;
    cout << endl;

    myset.clear();
    cout << "myset's size after clear: " << myset.size() << endl;

    myset.insert(1101);
    myset.insert(1102);

    cout << "myset now contains: ";
    for(set<int>::iterator it = myset.begin(); it != myset.end(); it++)
        cout << " " << *it;

    return 0;
}
```  
> 输出：  
> myset contains:  100 200 300 
> myset's size after clear: 0
> myset now contains:  1101 1102  
  

5. Operations相关函数：  
> **find**: Get iterator to element  
> **count**: Count elements with a specific value  
> **lower_bound**: Return iterator to lower bound  
> **upper_bound**: Return iterator to upper bound    
> **equal_range**: Get range of equal elements    
> 1. **find**: Searches the container for an element equivalent to val and returns **an iterator to it** if found, otherwise it returns an iterator to map::end. 
> Two elements of a set are considered equivalent if the container's comparison object returns false reflexively (i.e., no matter the order in which the elements are passed as arguments).
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;
    set<int>::iterator it;

    for(int i = 1; i< 6; i++)  myset.insert(i*10); //set: 10 20 30 40 50

    it = myset.find(20); // find函数返回的是iterator
    myset.erase(it); // erase by iterator
    myset.erase(myset.find(40));

    cout << "myset contains: ";
    for(it = myset.begin(); it != myset.end(); it++)
        cout << " " << *it;

    return 0;
}
```  
> 输出：   
> myset contains:  10 30 50

> 2. **count**: Count elements with a specific value. Searches the container for elements equivalent to val and returns **the number of matches**. 
> Because all elements in a set are unique, the function can only return 1(if the element is found) or zero(otherwise).
> Two elements of a set are considered equivalent if the container's comparison object returns false reflexively (i.e., no matter the order in which the elements are passed as arguments).
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;

    for(int i = 1; i < 5; i++)  myset.insert(i*3); //set: 3 6 9 12

    for(int i = 0; i < 10; i++){
        cout << i;
        if(myset.count(i) != 0)
            cout << " is an element of myset. \n";
        else
            cout << " is not an element of myset. \n";
    }
    
    return 0;
}
```  
> 输出：   
> 0 is not an element of myset.   
> 1 is not an element of myset.   
> 2 is not an element of myset.   
> 3 is an element of myset.   
> 4 is not an element of myset.   
> 5 is not an element of myset.   
> 6 is an element of myset.   
> 7 is not an element of myset.   
> 8 is not an element of myset.   
>9 is an element of myset. 

> 3. **lower_bound**: Return **iterator** to lower bound. Return an iterator pointing to the first element in the container which is not considered to go before val (i.e., either it is equivalent or goes after). 
> 4. **upper_bound**: Return **iterator** to upper bound. Return an iterator pointing to the first element in the container which is considered to go after val.
```
#include <set>
#include <iostream>
using namespace std;

int main(){
    set<int> myset;
    set<int>::iterator itlow, itup;

    for(int i = 1; i < 10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90

    itlow = myset.lower_bound(30); // [ 指向30
    itup = myset.upper_bound(60); // ( 指向70

    myset.erase(itlow,itup); //[30,70)之间的erase掉  10 20 70 80 90

    for(set<int>::iterator it = myset.begin(); it != myset.end(); it++)
        cout << " " << *it;

    return 0;
}
```  
> 输出：   
>  10 20 70 80 90


### 其他函数
refer to: http://www.cplusplus.com/reference/set/set/
