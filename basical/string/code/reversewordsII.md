题目描述：  
![image](/basical/string/image/image56.png)  
解题过程：  
看了code友的解答，先反转每个单词，再反转整个字符串，太妙了  
```cpp
class Solution {
public:
    void Swap(vector<char>& s, int left, int right) {
        while (left < right) {
            swap(s[left++],s[right--]);
        }
    }

    void reverseWords(vector<char>& s) {
        int left = 0, right = 0, ls = s.size();
        while (right < ls) {
            if (s[right] == ' ') {
                Swap(s,left,right-1);
                left = ++right;
            } else {
                right++;
            }
        }    
        Swap(s,left,ls - 1);
        Swap(s,0,ls - 1);
    }
};
```