题目描述：  
![image](/algorithmn/tracebak/image/image24.png)  
解决过程：  
自己没写出来，看了题解（看题解之前复习了一下[最长有效括号](/basical/string/code/longestvalidparathesis.md)）  
- 回溯：
    1. 计算出需要删减的左括号的个数lremove和右括号的个数rremove
    2. 从索引0位置开始进行回溯
    3. 回溯的时候有几个需要注意的点：首先是判断是否成功的条件语句，当lremove和rremove都为0时说明没有需要删除的左括号或者右括号；其次是因为删除一个左括号或者右括号存在重复性，即存在连续的左括号或者右括号，因此需要跳过重复字符；接着是如果剩余字符的数量小于待删除的字符的个数，可以直接退出（一种剪枝手法）
- 广度优先搜索：
    1. 不断地尝试：从删除1个左括号或者右括号开始，逐渐删除2个，3个……。当有一层遍历可以出现有效的字符串时说明找到了删除最少左括号或者右括号得到的有效字符串的所有可能  
代码：（回溯→广度优先搜索）  
```cpp
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        vector<string> ans;
        int lremove = 0, rremove = 0;
        for (char c : s) {
            if (c == '(') {
                lremove++;
            } else if (c == ')') {
                if (lremove == 0) rremove++;
                else lremove--;
            }
        }
        dfs(s,0,lremove,rremove,ans);
        return ans;
    }

    void dfs(conststring& s, int start, int lremove, int rremove, vector<string>& ans) {
        if (lremove == 0 && rremove == 0) {
            if (isvalid(s)) ans.push_back(s);
            return;
        }

        for (int i = start; i < s.length(); i++) {
            if (i > 0 && s[i] == s[i-1]) continue;
            if (lremove + rremove > s.length() - i) return;
            if (s[i] == '(') {
                dfs(s.substr(0,i) + s.substr(i+1), i, lremove-1, rremove, ans);
            } else if (s[i] == ')') {
                dfs(s.substr(0,i) + s.substr(i+1), i, lremove, rremove-1, ans);
            }
        }
    }

    bool isvalid(const string& str) {
        int cnt = 0;
        for (char c : str) {
            if (c == '(') {
                cnt++;
            } else if (c == ')') {
                if (cnt == 0) return false;
                else cnt--;
            }
        }
        return cnt == 0;
    }
};
```  
```cpp
class Solution {
public:
    bool isValid(string str) {
        int count = 0;

        for (char c : str) {
            if (c == '(') {
                count++;
            } else if (c == ')') {
                count--;
                if (count < 0) {
                    return false;
                }
            }
        }

        return count == 0;
    }

    vector<string> removeInvalidParentheses(string s) {
        vector<string> ans;
        unordered_set<string> currSet;

        currSet.insert(s);
        while (true) {
            for (auto & str : currSet) {
                if (isValid(str))
                    ans.emplace_back(str);
            }
            if (ans.size() > 0) {
                return ans;
            }
            unordered_set<string> nextSet;
            for (auto & str : currSet) {
                for (int i = 0; i < str.size(); i++) {
                    if (i > 0 && str[i] == str[i - 1]) {
                        continue;
                    }
                    if (str[i] == '(' || str[i] == ')') {
                        nextSet.insert(str.substr(0, i) + str.substr(i + 1, str.size()));
                    }
                }
            }
            currSet = nextSet;
        }
    }
};
```