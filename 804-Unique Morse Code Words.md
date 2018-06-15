# leetcode 804 - Unique Morse Code Words  
> 题目描述：https://leetcode.com/articles/unique-morse-code-words/  
#### 我的垃圾解法: 单纯利用vector  
> leetcode上提交的代码：  
```
struct MorsePairs{
    char letter;
    std::string morse;
};

class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        std::vector<std::string> answer;
    std::vector<std::string> morseCode = {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..",
                                          "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-",
                                          "-.--", "--..",".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..",
                                          "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-",
                                          "-.--", "--.."};
    MorsePairs mp;
    std::vector<MorsePairs> morseTransform; // moreseTransform存着52个字母和其摩斯码（vector<MorsePairs>类型）
    for (int i = 0; i <= 51; i++) {
        if(i < 26)
            mp.letter = char(i + 65);
        else
            mp.letter = char(i + 71);
        mp.morse = morseCode.at(i);
        morseTransform.push_back(mp);

    }
    for (int i = 0; i < words.size(); i++) { //对words里的每个单词
        string temp;
        for (int j = 0; j < words.at(i).length(); j++) { //对i-th单词的每个字母进行摩斯码转换
            for (int k = 0; k <= 51; k++) {
                if (words.at(i)[j] == morseTransform.at(k).letter) {
                    temp.insert(temp.length(),morseTransform.at(k).morse);
                }
            }
        }
        answer.push_back(temp);
    }
    for(int i = 0; i < answer.size(); i++){ //双循环查重，如果发现重复，用erase函数删去一个，最后vector里都是unique的，返回其size即可，注意erase函数的用法
        for(int j = i+1; j < answer.size(); j++) {
            if (answer.at(i) == answer.at(j)) {
                answer.erase(answer.begin() + j);
                j--;
            }
        }
    }
    return answer.size();
    }
};
```    
结果：  
AC  
但是效率很低。11ms，beats 3.44% of cpp submissions  

  
  #### 我的进阶解法: 利用vector，map和set  
> leetcode上提交的代码：  
```
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        vector<string> dots = {".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};

    map<char,string> letterToMorse;
    int k = 0;
    for(int i = 'a'; i <= 'z'; i++){
        letterToMorse[char(i)] = dots[k];
        k++;
    }

    set<string> myset;
    for(int i = 0; i < words.size(); i++){
        string word = words[i];
        string temp = " ";
        for(int j = 0; j < word.length(); j++){
            temp.insert(temp.length(),letterToMorse[char(word[j])]);
        }
        myset.insert(temp);
    }
    return myset.size() ;
    
    }
};
```    
结果：  
AC  
比上个方法有进步。 7ms，beats 46.65% of cpp submissions.
> 利用了map的键-值对特性，不需要自己构建struct  
> 利用了set的元素就是内容值本身，且元素unique的特性，轻易解决了差重的问题。

> 从这次代码中学到的／要提醒自己的：  
> 1. vector<struct>或者vector<string>里如果要添加元素，在for循环里赋值是会出错的，所以应该把要赋的值放在尖括号里同类型的temp里，然后vector.push_back(temp)!  
   >  2. 26字母的ascii码： a-z为 97-122； A-Z为 65-90.
   >  3. Hash真是个好东西。。。
