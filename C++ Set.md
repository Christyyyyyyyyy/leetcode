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

4. Modifiers相关函数：
> insert: Insert elements  
> erase: Erase elements  
> swap: Swap content  
> clear: Clear content  
> 1. **insert**: Extends the container by inserting new **elements**, effectively increasing the container **size** by the number of elements inserted.  
> Because element keys in a map are unique, the insertion operation checks whether each inserted element has a key equivalent to the one of an element already in the container, and if so, the element is not inserted, returning an iterator to this existing element (if the function returns a value).
> **An alternative way to insert elements in a map is by using member function map::operator[].**
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;
    mymap.insert(pair<char,int>('a',100));
    mymap.insert(pair<char,int>('z',200));

   for(auto& x: mymap){
        cout << x.first << ": " << x.second << endl;
    }
    return 0;
}
```  
> 输出：   
> a: 100    
> z: 200
> 2. **erase**: Removes from the map container either a single element or a range of elements([first,last])  
> This effectively reduces the container size by the number of elements removed, which are destroyed.  
>> 1) erase by iterator  
>> 2) erase by key  
>> 3) erase by range
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;
    map<char,int>::iterator it;

    //insert some values
    mymap['a'] = 10;
    mymap['b'] = 20;
    mymap['c'] = 30;
    mymap.insert(pair<char,int>('e',50));
    mymap.insert(pair<char,int>('f',60));
    mymap.insert(pair<char,int>('d',40));

    for(auto& x: mymap){
        cout << x.first << " : " << x.second << endl;
    }

    //erasing by iterator
    it = mymap.find('b');
    mymap.erase(it);

    //erasing by key
    mymap.erase('c');

    //erasing by range
    it = mymap.find('e');
    mymap.erase(it,mymap.end());

    //利用iterator 遍历 map
    for(it = mymap.begin(); it != mymap.end(); ++it){
        cout << it->first << " => " << it->second << endl;
    }
    return 0;
}
```  
> 输出：  
> a : 10  
> b : 20  
> c : 30  
> d : 40  
> e : 50  
> f : 60  
> a => 10  
> d => 40  

> 3. **swap**: Exchanges the content of the container by the content of x, which is another map of the same type. Sizes may differ.

> After the call to this member function, the elements in this container are those which were in x before the call, and the elements of x are those which were in this. All iterators, references and pointers remain valid for the swapped objects.
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> foo,bar;

    foo['x'] = 100;
    foo['y'] = 200;

    bar.insert(pair<char,int>('a',11));
    bar.insert(pair<char,int>('b',22));
    bar.insert(pair<char,int>('c',33));

    foo.swap(bar);

    cout << "foo contains: " << endl;
    for(auto& x: foo){
        cout << x.first << " => " << x.second << endl;
    }
    cout << "bar contains: " << endl;
    map<char,int>::iterator it;
    for(it = bar.begin(); it != bar.end(); it++){
        cout << it->first << " => " << it->second << endl;
    }
    
    return 0;
}
```  
> 输出：
> foo contains: 
> a => 11
> b => 22
> c => 33
> bar contains: 
> x => 100
> y => 200  

> 4. **clear**: Removes all elements from the map container (which are destroyed), leaving the container with a size of 0.

```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;

    mymap['x'] = 100;
    mymap['y'] = 200;
    mymap.insert(pair<char,int>('z',300));

    cout << "mymap contains: " << endl;
    map<char,int>::iterator it;
    for(it = mymap.begin(); it != mymap.end(); it++){
        cout << it->first << " => " << it->second << endl;
    }

    mymap.clear();
    cout << "size: " << mymap.size() << endl;

    mymap['a'] = 11;
    mymap.insert(pair<char,int>('b',22));

    for(auto&x:mymap){
        cout << x.first << " => " << x.second << endl;
    }
    return 0;
}
```  
> 输出：  
> mymap contains:   
> x => 100  
> y => 200  
> z => 300  
> size: 0  
> a => 11  
> b => 22  

5. Operations相关函数：  
> **find**: Get iterator to element  
> **count**: Count elements with a specific key  
> **lower_bound**: Return iterator to lower bound  
> **upper_bound**: Return iterator to upper bound    
> **equal_range**: Get range of equal elements    
> 1. **find**: Searches the container for an element with a key equivalent to k and returns **an iterator to it** if found, otherwise it returns an iterator to map::end. 
> Two keys are considered equivalent if the container's comparison object returns false reflexively (i.e., no matter the order in which the elements are passed as arguments).
> **Another member function, map::count, can be used to just check whether a particular key exists.**
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;
    map<char,int>::iterator it;

    mymap['a'] = 50;
    mymap['b'] = 100;
    mymap.insert(pair<char,int>('c',150));
    mymap.insert(pair<char,int>('d',200));

    it = mymap.find('b');
    if(it != mymap.end()){
        mymap.erase(it);
    }
    it = mymap.find('z');
    if(it == mymap.end()){
        cout << "can not find 'z'." << endl;
    }

    for(it = mymap.begin(); it != mymap.end(); it++){
        cout << it->first << " : " << it->second << endl;
    }

    return 0;
}
```  
> 输出：   
> can not find 'z'.   
> a : 50  
> c : 150  
> d : 200

> 2. **count**: Count elements with a specific key. Searches the container for elements with a key equivalent to k and returns **the number of matches**. 
> Because all elements in a map container are unique, the function can only return 1(if the element is found) or zero(otherwise).
> Two keys are considered equivalent if the container's comparison object returns false reflexively (i.e., no matter the order in which the elements are passed as arguments).
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;
    char c;

    mymap.insert(pair<char,int>('a',101));
    mymap['c'] = 202;
    mymap['f'] = 303;

    for(c = 'a'; c < 'h'; c++){
        cout << c;
        if(mymap.count(c) > 0)
            cout << " is an element of mymap." << endl;
        else
            cout << " is not an element of mymap." << endl;
    }
    return 0;
}
```  
> 输出：   
> a is an element of mymap.  
> b is not an element of mymap.  
> c is an element of mymap.  
> d is not an element of mymap.  
> e is not an element of mymap.  
> f is an element of mymap.  
> g is not an element of mymap.

> 3. **lower_bound**: Return **iterator** to lower bound. Return an iterator pointing to the first element in the container whose key is not considered to go before k (i.e., either it is equivalent or goes after). 
> 4. **upper_bound**: Return **iterator** to upper bound. Return an iterator pointing to the first element in the container whose key is considered to go after k.
```
#include <map>
#include <iostream>
using namespace std;

int main(){
    map<char,int> mymap;
    map<char,int>::iterator itlow,itup;

    mymap.insert(pair<char,int>('a',20));
    mymap['b'] = 20;
    mymap['c'] = 40;
    mymap['d'] = 80;
    mymap['e'] = 100;

    itlow = mymap.lower_bound('b');
    itup = mymap.upper_bound('d');

    mymap.erase(itlow,itup); //erases [itlow, itup)
    for(map<char,int>::iterator it = mymap.begin(); it != mymap.end(); it++){
        cout << it->first << " => " << it->second << endl;
    }

    return 0;
}
```  
> 输出：   
> a => 20
> e => 100


### 其他函数
refer to: http://www.cplusplus.com/reference/map/map/
