题目描述：

![image](/algorithmn/dynamic_programming/image/image96.png)

解决过程：

第366场周赛t3，没做出来

- 含代价的记忆化搜索

代码：（记忆化搜索→动态规划）

```cpp
class Solution {
public:
    int dp[520][520][2];
    int minOperations(string s1, string s2, int x) {
        int cnt1 = 0, cnt2 = 0;
        int n = s1.size();
        for (int i = 0; i < n; ++i) {
            cnt1 += s1[i] == '1';
            cnt2 += s2[i] == '1';
        }        
        if ((cnt1 % 2) != (cnt2 % 2)) return -1;
        memset(dp, -1, sizeof dp);
        function<int(int,int,bool)> dfs = [&](int i, int j, bool prev) -> int { // 判断0~i的子数组，此时还有j次翻转机会
            if (i < 0) {
                if (j > 0 || prev) return INT_MAX / 2;
                return 0;
            }
            if (dp[i][j][prev] != -1) return dp[i][j][prev];
            int &res = dp[i][j][prev];
            if ((s1[i] == s2[i]) == !prev) { // 第i个位置上s1和s2相等
                return res = dfs(i-1, j, false);
            }
            // 不相等
            int res1 = dfs(i-1, j + 1, false) + x; // 第一种方法
            int res2 = dfs(i-1, j, true) + 1; // 第二种方法
            res = min(res1, res2);
            if (j > 0) {
                res = min(res, dfs(i-1, j -1, false)); // 使用之前的翻转次数
            }
            return res;
        };
        return dfs(n - 1, 0, false);
    }
};
```

```cpp
class Solution {
public:
    int minOperations(string s1, string s2, int x) {
        if (s1 == s2) return 0;
        vector<int> p;
        for (int i = 0; i < s1.size(); i++)
            if (s1[i] != s2[i])
                p.push_back(i);
        if (p.size() % 2) return -1;
        int f0 = 0, f1 = x;
        for (int i = 1; i < p.size(); i++) {
            int new_f = min(f1 + x, f0 + (p[i] - p[i - 1]) * 2);
            f0 = f1;
            f1 = new_f;
        }
        return f1 / 2;
    }
};
```