题目描述：  
![image](/basical/string/image/image47.png)  
解决过程：  
一开始就想到的就是哈希表，写了一个比较丑陋的，而且比较暴力，然后改来改去逻辑终于简洁明了了：如果当前字母是小写字母且哈希表中存在对应大小字母或者当前字母是大写字母且哈希表中存在对应小写字母那么久更新答案  
题解里的哈希表很贪心，一开始就构造好了并且直接从大z字母开始往后找；还用了个位运算，我发现这些有关字母个数统计之类的题目都可以用位运算  
代码：（我的→题解哈希表→题解位运算）  
```cpp
class Solution {
public:
    string greatestLetter(string s) {
        string ans;
        unordered_set<char> set;
        char result = -1;
        for (char c : s) {
            if ((isupper(c) && set.count(tolower(c))) || (islower(c) && set.count(toupper(c)))) {
                char tmp = isupper(c) ? c : toupper(c);
                result = (result == -1) ? tmp : tmp > result ? tmp : result;
            }
            set.insert(c);
        }
        return result == -1 ? ans : ans + result;
    }
};
```

```cpp
class Solution {
public:
    string greatestLetter(string s) {
        unordered_set<char> ht(s.begin(), s.end());
        for (int i = 25; i >= 0; i--) {
            if (ht.count('a' + i) > 0 && ht.count('A' + i) > 0) {
                return string(1, 'A' + i);
            }
        }
        return "";
    }
};
```
```cpp
class Solution {
public:
    string greatestLetter(string s) {
        int lower = 0, upper = 0;
        for (auto c : s) {
            if (islower(c)) {
                lower |= 1 << (c - 'a');
            } else {
                upper |= 1 << (c - 'A');
            }
        }
        for (int i = 25; i >= 0; i--) {
            if (lower & upper & (1 << i)) {
                return string(1, 'A' + i);
            }
        }
        return "";
    }
};
```