题目描述：

![image](/basical/simulation/image/image8.png)

解决过程：

第355场周赛t1，ac

代码：

```cpp
class Solution {
public:
    vector<string> splitWordsBySeparator(vector<string>& words, char a) {
        int n = words.size();
        vector<string> ans;
        stack<string> stk;
        for (int i = 0; i < n; ++i) {
            string tmp = words[i];
            int j = 0, m = tmp.size();
            while (j < m) {
                int k = j;
                while (k < m && tmp[k] != a) {
                    ++k;
                }
                if (k == j) j++;
                else {
                    stk.push(tmp.substr(j, k - j));
                    j = k + 1;
                }
            }
        }
        while (!stk.empty()) {
            ans.push_back(stk.top());
            stk.pop();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```