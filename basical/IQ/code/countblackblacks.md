题目描述：

![image](/basical/IQ/image/image54.png)

解题过程：

第108场双周赛t4，没做出来

代码：

```
class Solution {
public:
    vector<long long> countBlackBlocks(int m, int n, vector<vector<int>>& coordinates) {
        unordered_set<long long> s;
        unordered_set<long long> d;
        for (const auto& p : coordinates) {
            int x = p[0], y = p[1];
            s.insert((long long)x * n + y);
            for (int dx = -1; dx <= 0; ++dx) {
                for (int dy = -1; dy <= 0; ++dy) {
                    int nx = x + dx, ny = y + dy;
                    if (nx >= 0 && nx < m - 1 && ny >= 0 && ny < n - 1) {
                        d.insert((long long)nx * n + ny);
                    }
                }
            }
        }
        vector<long long> a (5);
        a[0] = (long long)(m - 1) * (n - 1) - d.size();
        for (long long x : d) {
            int p = x / n;
            int q = x % n;
            int c = 0;
            c += s.count((long long)p * n + q);
            c += s.count((long long)p * n + q + 1);
            c += s.count((long long)(p + 1) * n + q);
            c += s.count((long long)(p + 1) * n + q + 1);
            a[c]++;
        }
        return a;
    }
};
```