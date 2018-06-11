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
    std::vector<MorsePairs> morseTransform;
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
        for (int j = 0; j < words.at(i).length(); j++) { //对i-th单词的每个字母进行莫斯转换
            for (int k = 0; k <= 51; k++) {
                if (words.at(i)[j] == morseTransform.at(k).letter) {
                    temp.insert(temp.length(),morseTransform.at(k).morse);
                }
            }
        }
        answer.push_back(temp);
    }
    for(int i = 0; i < answer.size(); i++){
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
> 从这次代码中
