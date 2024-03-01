题目描述：

![image](/basical/array/image/image116.png)

解决过程：
没做出来

- 滑动窗口

代码：

```
class Solution {
public:
    int takeCharacters(string s, int k) {
        int n = s.size(), j = n;
        unordered_map<char,int> cnt;
        while (cnt['a'] < k || cnt['b'] < k || cnt['c'] < k) {
            if (j == 0) return -1;
            j -= 1;
            cnt[s[j]]++;
        }
        // cout << "here" << endl;
        int ans = n - j;
        for (int i = 0; i < n; ++i) {
            cnt[s[i]]++;
            while (j < n && cnt[s[j]] > k) {
                cnt[s[j]]--;
                j++;
            }
            ans = min(ans, i + 1 + n - j);
            // cout << i << ' ' << j << ' ' << ans << endl;
            if (j == n) break;
        }
        return ans;
    }
};
```