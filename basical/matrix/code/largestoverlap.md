题目描述：

![image](/basical/gragh/image/image19.png)

解题过程：

没做出来，纯思维题

代码：

```cpp
class Solution {
public:
    int largestOverlap(vector<vector<int>>& img1, vector<vector<int>>& img2) {
        int n = img1.size();
        vector<vector<int>> cnt (2*n+1, vector<int> (2*n+1, 0));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (img1[i][j] == 1) {
                    for (int ii = 0; ii < n; ++ii) {
                        for (int jj = 0; jj < n; ++jj) {
                            if (img2[ii][jj] == 1) {
                                cnt[i - ii + n][j -jj + n]++;
                            }
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (const auto& v : cnt) {
            for (int x : v) {
                ans = max(ans, x);
            }
        }
        return ans;
    }
};
```