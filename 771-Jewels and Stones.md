# leetcode-771-Jewels and Stones
最简单的题目，双循环遍历。
> leetcode上提交的代码：  
 ```
 class Solution {
public:
    int numJewelsInStones(string J, string S) {
    int num = 0;
    for(int i = 0; i < S.length(); i++){
        for(int j = 0; j < J.length(); j++){
            if(S[i] == J[j]){
                num++;
            }
        }
    }
    return num; 
    }
};
 ```
 >CLion上自测的代码：  
 >Solution.h:  
 ```
#ifndef LEETCODE_JEWELS_AND_STONES_SOLUTION_H
#define LEETCODE_JEWELS_AND_STONES_SOLUTION_H

#include <string>

class Solution {
public:
    int numJewelsInStones(std::string J, std::string S);
};

#endif
 ```  
 >Solution.cpp:
 ```
 #include "Solution.h"
int Solution::numJewelsInStones(std::string J, std::string S) {
    int num = 0;
    for(int i = 0; i < S.length(); i++){
        for(int j = 0; j < J.length(); j++){
            if(S[i] == J[j]){
                num++;
            }
        }
    }
    return num;
}
 ```  
 >main.cpp:  
 ```
 int main() {
    Solution sol;
    cout << sol.numJewelsInStones("z","ZZ") << endl;
    return 0;
}
 ```
 复习：  
 c++中string.length()可返回string的长度  
 可用str[]来访问string中的元素
