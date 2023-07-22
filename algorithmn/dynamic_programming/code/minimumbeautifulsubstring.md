题目描述：

![image](/algorithmn/dynamic_programming/image/image79.png)

解决过程：

第108场双周赛t3，1：02

代码：

```cpp
unordered_set<string> ss;
unordered_set<int> p;
int init = []() {
    for (int i = 0; i < 7; ++i) {
        int x = 1, j = i;
        while (j) {
            x *= 5;
            j--;
        }
        string temp;
        while (x) {
            if (x & 1) {
                temp += '1';
            } else {
                temp += '0';
            }
            x = x >> 1;
        }
        reverse(temp.begin(), temp.end());
        ss.insert(temp);
        p.insert(temp.size());
    }
    return 0;
}();
class Solution {
public:
    int memo[20][20];
    int minimumBeautifulSubstrings(string s) {
        int n = s.size();
        int ans = 0;
        // for (auto x : ss) cout << x << endl;
        memset(memo, -1, sizeof memo);
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            // cout << i << ' ' << j << endl;
            if (i == j) {
              return s[i] == '1' ? 1 : 0;  
            } else if (memo[i][j] != -1) {
                // cout << "find" << endl;
                return memo[i][j];
            } else if (p.count(j - i + 1) && ss.count(s.substr(i, j - i + 1))) {
                // cout << "has" << endl;
                return 1;
            }
            int res = 0x3f3f3f3f;
            for (int k = i; k < j; ++k) {
                // cout << "now k is : " << k << endl;
                int f = dfs(i, k);
                if (f == 0) continue;
                int a = dfs(k + 1, j);
                if (a == 0) continue;
                res = min(res, f + a);
                // cout << i << ' ' << k  << ' ' << j << ' ' << res << endl;
            }
            return memo[i][j] = (res == 0x3f3f3f3f ? 0 : res);
        };
        ans = dfs(0, n - 1);
        return ans == 0 ? -1 : ans;
    }
};
```