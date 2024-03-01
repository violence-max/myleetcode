题目描述：

![image](/basicaldatastructure/binary_tree/image/image68.png)

解决过程：

ac，但是太墨迹了，没发现是lca

代码：（我的→灵神→位运算）

```cpp
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>>& queries) {
        // __builtin_clz(x)可以计算出x的二进制中前导0的数量
        function<int(int)> getheight = [&](int x) -> int {
            int ret = 0;
            while (x) {
                ret++;
                x = x >> 1;
            }
            return ret - 1;
        };
        int m = queries.size();
        vector<int> ret(m);
        for (int i = 0; i < m; ++i) {
            int a = queries[i][0], b = queries[i][1];
            int h1 = getheight(a), h2 = getheight(b);
            if (h1 > h2) {
                swap(a, b);
                swap(h1, h2);
            }
            int diff = h2 - h1;
            while (diff--) {
                b /= 2;
            }
            int h = 0;
            while (a != b) {
                h++;
                a /= 2;
                b /= 2;
            }
            ret[i] = 2 * h + h2 - h1 + 1;
        }
        return ret;
    }
};
```

```cpp
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>> &queries) {
        int m = queries.size();
        vector<int> ans(m);
        for (int i = 0; i < m; ++i) {
            int res = 1, a = queries[i][0], b = queries[i][1];
            while (a != b) {
                a > b ? a /= 2 : b /= 2;
                ++res;
            }
            ans[i] = res;
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>> &queries) {
        int m = queries.size();
        vector<int> ans(m);
        for (int i = 0; i < m; ++i) {
            int a = queries[i][0], b = queries[i][1];
            if (a > b) swap(a, b); // 保证 a <= b
            int d = __builtin_clz(a) - __builtin_clz(b);
            b >>= d; // 上跳，和 a 在同一层
            ans[i] = a == b ? d + 1 : d + (32 - __builtin_clz(a ^ b)) * 2 + 1;
        }
        return ans;
    }
};
```