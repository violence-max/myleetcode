题目描述：

![image](/algorithmn/dynamic_programming/image/image99.png)

解决过程：

第368场周赛t4，没做出来

- 划分型dp

代码：

```cpp
const int MX = 201;
vector<vector<int>> d (MX);
int init = []() {
    for (int i = 1; i < MX; ++i) {
        for (int j = 2 * i; j < MX; j += i) {
            d[j].push_back(i);
        }
    }
    return 0;
}();

class Solution {
public:
    int get_modify(const string& str) {
        int n = str.size();
        int res = INT_MAX;
        for (auto dd : d[n]) {
            int cnt = 0;
            for (int i = 0; i < dd; ++i) {
                string ns = "";
                for (int j = i; j < n; j += dd) {
                    ns += str[j];
                }
                for (int left = 0; left < ns.size() / 2; ++left) {
                    cnt += ns[left] != ns[ns.size()-1-left];
                }
            }
            res = min(res, cnt);
        }
        return res;
    }

    int m[201][201];
    int f[201];
    int minimumChanges(string s, int k) {
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                m[i][j] = get_modify(s.substr(i, j - i + 1));
            }
        }

        for (int i = 0; i < n; ++i) f[i] = m[0][i];
        for (int i = 1; i < k; ++i) {
            for (int j = n - 1; j > 2 * i; --j) {
                int res = INT_MAX;
                for (int l = 2 * i; l < j; ++l) {
                    res = min(res, f[l-1] + m[l][j]);
                }
                f[j] = res;
            }
        }
        return f[n-1];
    }
};
```