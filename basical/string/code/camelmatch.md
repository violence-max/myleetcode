题目描述：  
![image](/basical/string/image/image68.png)  
解决过程：  
- 双指针：
1. 如果pattern对于queries[i]可以满足pattern是queries[i]的子序列而且去掉pattern之后queries[i]全部都是小写字母就可以满足查询条件  
代码：  
```cpp
class Solution {
public:
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        int n = queries.size();
        vector<bool> res (n, true);
        for (int i = 0; i < n; i++) {
            int p = 0;
            for (auto c : queries[i]) {
                if (p < pattern.size() && pattern[p] == c) {
                    p++;
                } else if (isupper(c)) {
                    res[i] = false;
                    break;
                }
            }
            if (p < pattern.size()) {
                res[i] = false;
            }
        }
        return res;
    }
};
```