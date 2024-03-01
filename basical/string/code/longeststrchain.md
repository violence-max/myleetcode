题目描述：  
![image](/basical/string/image/image70.png)  
解决过程：  
没做出来  
代码：动态规划  
```cpp
class Solution {
public:
    int longestStrChain(vector<string>& words) {
        // 自定义排序，将words中长度较小的字符串放到前面
        sort(words.begin(), words.end(), [&](string a, string b) {
            return a.size() < b.size();
        });
        int res = 0;
        unordered_map<string, int> cnt; // 记录以字符串word为结尾的字符串链的最长长度
        for (auto word : words) {
            cnt[word] = 1;
            for (int i = 0; i < word.size(); i++) {
                string prev = word.substr(0, i) + word.substr(i + 1); // 删除编号为i的字符得到字符串链中的之前的串
                if (cnt.count(prev)) {
                    cnt[word] = max(cnt[word], cnt[prev] + 1);
                }
            }
            res = max(res, cnt[word]);
        }
        return res;
    }
};
```