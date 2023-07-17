题目描述：

![image](/basical/IQ/image/image52.png)

解决过程：

没做出来

暴力枚举每个状态就可以过

代码：

```cpp
class Solution {
public:
    int maximumGood(vector<vector<int>>& statements) {
        int n = statements.size();
        int ans = 0;
        for (int m = 1; m < (1 << n); ++m) {
            bool find = true;
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (i == j) continue;
                    if (statements[i][j] == 1) {
                        if ((m & (1 << i)) && !(m & (1 << j))) {
                            find = false;
                            break;
                        }
                    } else if (statements[i][j] == 0) {
                        if ((m & (1 << i)) && (m & (1 << j))) {
                            find = false;
                            break;
                        }
                    }
                }
                if (!find) break;
            }
            if (find) ans = max(ans, __builtin_popcount(m));
        }
        return ans;
    }
};
```