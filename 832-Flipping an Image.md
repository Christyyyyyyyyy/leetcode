# leetcode-832-Flipping an Image
> 题目描述：https://leetcode.com/problems/flipping-an-image/description/  

#### 我的最终提交解法:   
> leetcode上提交的代码：  
```
 class Solution {
public:
    vector<vector<int>> flipAndInvertImage(vector<vector<int>>& A) {
       for(std::vector<std::vector<int>>::iterator i = A.begin(); i != A.end(); ++i){
        std::vector<int>::iterator m = (*i).begin();
        std::vector<int>::iterator k = (*i).end()-1;

        for(int myint = 0; myint < (*i).size()/2; myint++){
            *m = (*m) ^ 1;
            *k = (*k) ^ 1;
            swap(*m,*k);
            m += 1;
            k -= 1;
        }
        if((*i).size() % 2 != 0)
            *m = (*m) ^ 1;
    }
    return A;
    }
};
```    
结果：  
AC  
14ms  

从这次代码中学到的／要提醒自己的：  
1. 二维vector的遍历方法（上述代码中的2个for）   
```
for(vector<vector<int>>::iterator i = A.begin(); i != A.end(); ++i){
    for(int myint = 0; myint < (*i).size(); myint++){  
    }  
}
```    
 然后用*i，*m，*k分别指代不同的内容哦。   

2. 只有0，1是布尔型是，～0 = 1， ～1 = 0 成立。  
如果1是int形式，那么～1 不等于 0，而是等于 -2; ~0 等于 -1     
如果1是int形式，那么～1 不等于 0，而是等于 -2; ~0 等于 -1       
在计算机中存储的信息均是已二进制形式保存的。且数字是以补码形式保存的，正数的补码和原码同，负数的补码为原码取反后加1  
1:   0000 0000 0000 0001    
～1: 1111 1111 1111 1110    
上面～1变为十进制： 我们知道在高位为1时表示该数是负数。把负数二进制转为十进制步骤就是将其二进制减一，再取反（注意符号位不变任为1）    
结果：1111 1111 1111 1101  
      1000 0000 0000 0010    
第一个1表示负数，剩下的0，1转换为十进制是2，所以～！= -2    

3. 在1->0;0->1的转换里，我试了好几种，最后也只是把运行时间从15ms 提升到 14ms。。。  
```
*m = ~(*m); 
*k = ~(*k);
```  
> 这是最开始的做法，以为 ～0 == 1； ～1 == 0；然而是错的，原因上面说了。
```
if(*m == 0) *m = 1; else *m = 0; //15ms
if(*k == 0) *k = 1; else *k = 0;
```  
> 这是电脑快没电前没想到其他方法，只能用if判断
```
*m = (*m) ^ 1; // 14ms
*k = (*k) ^ 1;
```  
> 这是参考了submission里的方法，与1进行异或，效果是:k^1,如果k是偶数，则使k=k+1；若k是奇数，则使k=k-1。
```
*m = (*m + 1) % 2; // 15ms
*k = (*k + 1) % 2;  
```  
>这也是参考了submission里的方法，我没有想到 %2 这个方法
```
*m = ~(*m) + 2;// 15ms
*k = ~(*k) + 2;
```  
>这个是后面想出来的，就是刚好利用～1 = -1； ～0 = -2；所以都+2可以得到我们想要的




                                           
