题目描述：  
![image](/algorithmn/tracebak/image/image3.png)  
解题过程：  
看了题解才会做哈哈。一开始的迷惑的地方在于如何构造哈希表，我以为有遍历a~z的很简便的写法，python应该就有。但是就算有哈希表，好像我也没有想出来应该怎么做吧哈哈哈。题解就是简单回溯而已，简单一批。  
代码：  
```cpp
class Solution {
private:
    unordered_map<char,string> phoneMap;
    vector<string> res;
    string temp;
public:
    void dfs(string digits,int cur) {
        if (temp.size() == digits.size()) {
            res.push_back(temp);
            return;
        }
        string letter = phoneMap[digits[cur]];
        for (auto c : letter) {
            temp.push_back(c);
            dfs(digits,cur+1);
            temp.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if (digits.size() == 0) return res;
        phoneMap = {
            {'2',"abc"},
            {'3',"def"},
            {'4',"ghi"},
            {'5',"jkl"},
            {'6',"mno"},
            {'7',"pqrs"},
            {'8',"tuv"},
            {'9',"wxyz"}
        };
        dfs(digits,0);
        return res;
```