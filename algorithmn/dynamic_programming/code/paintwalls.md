题目描述：

![image](/algorithmn/dynamic_programming/image/image68.png)

解决过程：
第350场周赛t4

- 回溯+记忆化搜索
- 0-1背包问题

代码：（回溯+记忆化搜索→0-1背包问题）

```cpp
class Solution {
public:
    int n;
    int dfs(int i, int j, const vector<int>& cost, const vector<int>& time, vector<vector<int>>& d) {
        // cout << i << ' ' << j << endl;
        // 意义：当前累计付费刷墙的时间总和为j，需要刷下标0~i的墙
        if (j > i) {
            // 累计付费刷墙时间可以兑换成1单位的免费刷墙时间，由于大于剩余需要刷的墙的总数，因此开销免费的，即0
            return 0;
        }
        if (i < 0) {
            // 会由i == 0的状态转移过来，此时返回最大值即可，会被min函数排除掉
            return INT_MAX / 2;
        }
        if (d[i][j + n] != -1) {
            return d[i][j + n];
        }
        // 分别对应于第i面墙是付费刷还是免费刷
        
        return d[i][j + n] = min(dfs(i - 1, j + time[i], cost, time, d) + cost[i], dfs(i - 1, j - 1, cost, time, d));
    }

    int paintWalls(vector<int>& cost, vector<int>& time) {
        n = time.size();
        vector<vector<int>> d (n, vector<int> ((n << 1) + 1, -1));
        return dfs(n - 1, 0, cost, time, d);    
    }
};
```

```cpp
class Solution {
public:
    int paintWalls(vector<int>& cost, vector<int>& time) {
        // 0-1背包问题
        // 付费墙的总数 + 免费墙的总数 == n
        // 贪心的思路：付费墙的时间 >= 免费墙的总数
        // 得到：付费墙的时间 >= n - 付费墙的总数
        // 即：付费墙的时间 + 付费墙的总数 >= n
        // 假设付费墙一共有3面，则以上不等式可以转换为：
        // t1 + t2 + t3 + 3 >= n, 即 (t1 + 1) + (t2 + 1) + (t3 + 1) >= n
        // 即可转换为至少型的0-1背包问题：体积（付费墙时间）至少为n的情况下求每面墙价值最低

        int n = cost.size();
        // f[i][j]表示刷完0~i面墙剩余需要的体积为j
        // vector<vector<int>> f (n + 1, vector<int> (n + 1, INT_MAX / 2));
        // for (int i = 0; i <= n; ++i) {
        //     f[i][0] = 0;
        // }
        // for (int i = 1; i <= n; ++i) {
        //     int c = cost[i-1], t = time[i-1] + 1
        //     for (int j = n; j >= 1; j--) {
        //             f[i][j] = min(f[i-1][j - t] + c, f[i-1][j]);
        //             cout << i << ' ' << j << ' ' << f[i][j] << endl;
        //     }
        // }
        // return f[n][n];
        vector<int> f (n + 1, INT_MAX / 2);
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            int c = cost[i], t = time[i] + 1;
            for (int j = n; j >= 1; --j) {
                f[j] = min(f[max(0, j - t)] + c, f[j]);
                // cout << i << ' ' << j << ' ' << f[j] << endl;
            }
        }
        return f[n];
    }
};
```