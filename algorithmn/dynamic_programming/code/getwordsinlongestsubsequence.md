题目描述：

![image](/algorithmn/dynamic_programming/image/image97.png)

解决过程：

第115场双周赛t3，没做出来

- 序列式dp

代码：

```cpp
class Solution {
public:
    bool ok(const string& s1, const string& s2) {
        if (s1.size() != s2.size()) return false;
        bool diff = false;
        int n = s1.size();
        for (int i = 0; i < n; ++i) {
            if (s1[i] != s2[i]) {
                if (diff) return false;
                diff = true;
            }
        }
        return true;
    }

    int f[1020]; // f[i]定义为从下标从i到n-1的区间内构成的满足条件的最长子序列的长度
    int from[1020];
    vector<string> getWordsInLongestSubsequence(int n, vector<string>& ws, vector<int>& gs) {
        int mx = n - 1;
        memset(f, 0, sizeof f);
        memset(from, -1, sizeof from);
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (f[j] > f[i] && gs[i] != gs[j] && ok(ws[i], ws[j])) {
                    f[i] = f[j];
                    from[i] = j;
                }
            }
            f[i]++;
            if (f[i] > f[mx]) {
                mx = i;
            }
        }
        int m = f[mx];
        vector<string> ans (m);
        for (int i = 0; i < m; ++i) {
            ans[i] = ws[mx];
            mx = from[mx];
        }
        return ans;
    }
};
```