# leetcode Two Sum
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**
> Given nums = [2, 7, 11, 15], target = 9,  
Because nums[0] + nums[1] = 2 + 7 = 9,  
return [0, 1].

#### 解法1:暴力解法
两个嵌套的for循环。  
第一个for循环遍历数组的每一个元素，对于当前的元素，第二个for循环则遍历数组中的所有其他元素，寻找是否有与之满足条件的数。  
这个算法的时间复杂度为O(n^2)，空间复杂度为O(1)。  
> leetcode上提交的代码
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> index;
        for(int i = 0; i < nums.size(); i++){
            for(int j = i+1; j < nums.size(); j++){
                if(nums.at(i) + nums.at(j) == target){
                    index.push_back(i);
                    index.push_back(j);
                }
            }
        }
        return index;
    }
};
```
结果：  
AC  
Your runtime beats 11.12 % of cpp submissions.  
>>注意，如果是在CLion上调试的话：  
main.cpp:
```
#include <vector>
#include "Solution.h"
using namespace std;

int main(){
    vector<int> input,output;
    input.push_back(2);
    input.push_back(3);
    input.push_back(4);
    input.push_back(5);
    input.push_back(6);
    input.push_back(5);

    Solution sol1;
    output = sol1.twoSum(input,8);

    for(int k = 0; k < output.size(); k++){
        cout << output[k] << endl;
    }

    return 0;
 }
```  
Solution.h:
```
#ifndef LIANSHOU_SOLUTION_H
#define LIANSHOU_SOLUTION_H

#include <vector>

class Solution {
public:
    std::vector<int> twoSum(std::vector<int>& data, int target);
};

#endif 
```  
Solution.cpp:  
```
#include "Solution.h"
#include <iostream>
#include <vector>
using namespace std;

vector<int> Solution::twoSum(vector<int>& data, int target){
    vector<int> index;
    for (int i = 0; i < data.size(); i++){
        for(int j = i + 1; j < data.size(); j++){
            if(data.at(i) + data.at(j) == target){
                index.push_back(i);
                index.push_back(j);
            }
        }
    }
    return index;
}
```
#### 解法2:双指针法  
> v1:  
```
class Solution {
public:
    vector<int> twoSum(vector<int>& data, int target) {
        vector<int>cdata (data); //constructor of vector: cdata is a copy of data
    vector<int>index; //放答案
    partial_sort_copy(data.begin(),data.end(),cdata.begin(),cdata.end()); //该函数对部分元素进行排序，并把结果复制到另一个容器中
    int left = 0, right = cdata.size() - 1; //双下标
    int result = cdata.at(left) + cdata.at(right);
    while(right > left){
        if(result == target){
            int pos = find(data.begin(), data.end(), cdata.at(left)) - data.begin(); //猜想：find函数返回的是类似于begin()的类指针，为了得到index就要减去最开始的指针
            index.push_back(pos);
            index.push_back(find(data.begin(),data.end(),cdata.at(right)) - data.begin());
            left++;
        }
        else if(result > target){
            right--; //向前移动下标
        }
        else{
            left++; //向后移动上标
        }
        result = cdata.at(left) + cdata.at(right);
    }

    return index;
    }
};
```  
> 结果：  
> WA  
> 15 / 20 test cases passed.  
> Input:[3,3] 6  
> Output:[0,0]  
> Expected: [0,1]  
> 错误原因： 在将原数组排序并复制的时候，如果有两个值相同的元素，find函数只会取第一个。
>> 怎么办呢。。。   
>>> 来试试vector&lt;struct&gt;好了。  
  
  > v2:  
```
struct Pair{
    int num;
    int ind;
}mypair;

static bool LessSort(Pair p1, Pair p2) {
    return p1.num < p2.num;
}

class Solution {
public:
    vector<int> twoSum(vector<int>&data, int target) {
    vector<Pair>cdata;
    Pair mypair;
    for(int i = 0; i < data.size(); i++){
        mypair.num = data.at(i);
        mypair.ind = i;
        cdata.push_back(mypair);
    }//把data传入cdata中，num存元素，ind存下标

    vector<int>index; //放答案

    sort(cdata.begin(),cdata.end(),LessSort);

    int left = 0, right = cdata.size() - 1; //双下标
    int result = cdata.at(left).num + cdata.at(right).num;
    while(right > left){
        if(result == target){
            int pos = cdata.at(left).ind; //猜想：find函数返回的是类似于begin()的类指针，为了得到index就要减去最开始的指针
            index.push_back(pos);
            index.push_back(cdata.at(right).ind);
            left++;
        }
        else if(result > target){
            right--; //向前移动下标
        }
        else{
            left++; //向后移动上标
        }
        result = cdata.at(left).num + cdata.at(right).num;
    }
    return index;
    }
};
```  
> 结果：  
> AC  
> Runntime:9ms, beats 92.29% of cpp submissions      
>>注意，如果是在CLion上调试的话：  
main.cpp:
```
#include<iostream>
#include <vector>
#include "Solution.h"

using namespace std;


int main(){
    vector<int> input,output;
    input.push_back(2);
    input.push_back(2);
    input.push_back(0);
    input.push_back(0);
    input.push_back(0);
    input.push_back(0);

    Solution sol1;
    output = sol1.twoSum(input,4);

    for(int k = 0; k < output.size(); k++){
        cout << output[k] << endl;
    }
    
    return 0;
 }

```  
Solution.h:
```
#ifndef LIANSHOU_SOLUTION_H
#define LIANSHOU_SOLUTION_H

#include <vector>

struct Pair{
    int num;
    int ind;
};

static bool LessSort(Pair p1, Pair p2) {
    return p1.num < p2.num;
}

class Solution {
public:
    std::vector<int> twoSum(std::vector<int>& data, int target);

};

#endif 
```  
Solution.cpp:  
```
#include "Solution.h"
#include <iostream>
#include <vector>
using namespace std;

vector<int> Solution::twoSum(vector<int>& data, int target){
    vector<Pair>cdata;
    Pair mypair;
    for(int i = 0; i < data.size(); i++){
        mypair.num = data.at(i);
        mypair.ind = i;
        cdata.push_back(mypair);
    }//把data传入cdata中，num存元素，ind存下标

    vector<int>index; //放答案

    sort(cdata.begin(),cdata.end(),LessSort);

//    partial_sort_copy(data.begin(),data.end(),cdata.begin(),cdata.end()); //该函数对部分元素进行排序，并把结果复制到另一个容器中
    
    int left = 0, right = cdata.size() - 1; //双下标
    int result = cdata.at(left).num + cdata.at(right).num;
    while(right > left){
        if(result == target){
            int pos = cdata.at(left).ind; //猜想：find函数返回的是类似于begin()的类指针，为了得到index就要减去最开始的指针
            index.push_back(pos);
            index.push_back(cdata.at(right).ind);
            left++;
        }
        else if(result > target){
            right--; //向前移动下标
        }
        else{
            left++; //向后移动上标
        }
        result = cdata.at(left).num + cdata.at(right).num;
    }
    return index;
}
```
