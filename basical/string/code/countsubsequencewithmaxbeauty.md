题目描述：

![image](/basical/string/image/image86.png)

解决过程：
第112场双周赛t4，没做出来

- 组合数学

代码：

```cpp
int c[27][27];
auto init = []() -> int {
    c[0][0] = 1;
    for (int i = 1; i <= 26; ++i) {
        c[i][0] = c[i][i] = 1;
        for (int j = 1; j < i; ++j) {
            c[i][j] = c[i-1][j-1] + c[i-1][j];
            // cout << i << ' ' << j << ' ' << c[i][j] << endl;
        }
    }
    return 0;
}();

class Solution {
public:
    int mod = 1e9 + 7;
    int cnt[26];
    int countKSubsequencesWithMaxBeauty(string s, int k) {
        if (k > 26) return 0;

        memset(cnt, 0 ,sizeof cnt);
        for (char c : s) cnt[c-'a']++;
        int id[26];
        iota(id, id + 26, 0);
        sort(id, id + 26, [&](int a, int b){
            return cnt[a] > cnt[b];
        });
        if (cnt[id[k-1]] == 0) return 0;
        int left = k - 1, right = k - 1;
        while (left > 0 && cnt[id[left-1]] == cnt[id[left]]) --left;
        while (right < 25 && cnt[id[right]] == cnt[id[right+1]]) ++right;
        long long aug = 1;
        for (int i = 0; i < k; ++i) {
            aug = aug * cnt[id[i]] % mod;
        }
        // cout << aug << ' ' << left << ' ' << right << endl;
        int l = right - left + 1;
        // cout << c[l][k-left] << endl;
        return aug * c[l][k-left] % mod;
    }
};
```