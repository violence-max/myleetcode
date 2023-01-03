题目描述：  
![image](/basical/string/image/image36.png)  
我胡汉三又回来了！！！  
休息了这么多天终于痊愈了，芜湖起飞。最近开始一天四题计划，计划把之前四天没有做的全部补上。  
这道题比较弱智感觉，就是一个分词，但是 字符串的数字不可以直接拿来比较，要转换为整数才行（学会了stoi这个api）  
就不仔细描述算法步骤了  
代码：  
```cpp
class Solution {
public:
    bool areNumbersAscending(string s) {
        auto split = [](const string & str , char delim) -> vector<string> {
            vector<string> ans;
            string cur;
            for (auto ch : str) {
                if (ch == delim) {
                    ans.push_back((move(cur)));
                    cur.clear();
                }
                else {
                    cur += ch;
                }
            }
            ans.push_back(move(cur));
            return ans;
        };

        vector<string> ans = split(s,' ');
        int MAX = -1;
        for (auto s : ans) {
            if (s[0] >= 'a' && s[0] <= 'z')
                continue;
            else {
                int number = stoi(s,0,10);
                if (number <= MAX) return false;
                else MAX = number;
            }
        }
        return true;
    }
};
```
```cpp
class Solution {
public:
    bool areNumbersAscending(string s) {
        int pre = 0, pos = 0;
        while (pos < s.size()) {
            if (isdigit(s[pos])) {
                int cur = 0;
                while (pos < s.size() && isdigit(s[pos])) {
                    cur = cur * 10 + s[pos] - '0';
                    pos++;
                }
                if (cur <= pre) {
                    return false;
                }
                pre = cur;
            } else {
                pos++;
            }
        }
        return true;
    }
};
```