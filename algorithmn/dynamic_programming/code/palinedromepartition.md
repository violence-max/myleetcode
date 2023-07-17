题目描述：

![image](/algorithmn/dynamic_programming/image/image77.png)

解题过程：

写得比较保守了，写完还思考了好久，最后还是轻松过了

- 深度优先搜索

代码：（dfs）

```cpp
class Solution {
public:
    int m[120][120];
    int palindromePartition(string s, int k) {
        int n = s.size();
        unordered_map<int, string> re;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int index = i * n + j;
                re[index] = s.substr(i, j - i + 1);
            }
        }
        memset(m, -1, sizeof m);
        function<int(int,int)> get = [&](int s, int i) -> int {
            int index = s * n + i;
            string tmp = re[index];
            int l = 0, r = tmp.size() - 1;
            int c = 0;
            while (l < r) {
                if (tmp[l] != tmp[r]) c++;
                l++;
                r--;
            }
            return c;
        };
        function<int(int, int)> dfs = [&](int s, int cnt) -> int {
            if (m[s][cnt] != -1) return m[s][cnt];

            if (cnt == k - 1) {
                return get(s, n - 1);
            } else {
                int ret = INT_MAX;
                for (int i = s; i <= n - k + cnt; ++i) {
                    int c = get(s, i);
                    // cout << s << ' ' << i << ' ' << c << endl;
                    m[i+1][cnt+1] = dfs(i + 1, cnt + 1);
                    c += m[i+1][cnt+1];
                    // cout << ' ' << m[i+1][cnt+1] << ' ' << c << endl;
                    ret = min(ret, c);
                    // cout << s << ' ' << i << ' ' << ret << endl;
                }
                return ret;
            }
        };
        return dfs(0, 0);
    }
};
```

```cpp
class Solution {
public:
    int palindromePartition(string& s, int k) {
        int n = s.size();
        vector<vector<int>> cost(n, vector<int>(n));
        for (int span = 2; span <= n; ++span) {
            for (int i = 0; i <= n - span; ++i) {
                int j = i + span - 1;
                cost[i][j] = cost[i + 1][j - 1] + (s[i] == s[j] ? 0 : 1);
            }
        }

        vector<vector<int>> f(n + 1, vector<int>(k + 1, INT_MAX));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= min(k, i); ++j) {
                if (j == 1) {
                    f[i][j] = cost[0][i - 1];
                }
                else {
                    for (int i0 = j - 1; i0 < i; ++i0) {
                        f[i][j] = min(f[i][j], f[i0][j - 1] + cost[i0][i - 1]);
                    }
                }
            }
        }
        
        return f[n][k];
    }
};
```