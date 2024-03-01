题目描述：

![image](/basical/IQ/image/image59.png)

解决过程：

没做出来

- 枚举字符

代码：

```
class Solution {
public:
    int minCharacters(string a, string b) {
        int n = a.size(), m = b.size();
        int cnt1[26]{}, cnt2[26]{};
        for (auto c : a) cnt1[c-'a']++;
        for (auto c : b) cnt2[c-'a']++;
        int ans = INT_MAX;
        for (int i = 0; i < 26; ++i) {
            ans = min(ans, n - cnt1[i] + m - cnt2[i]);
            if (i == 0) continue;
            int r1 = 0, r2 = 0;
            for (int j = i; j < 26; ++j) r1 += cnt1[j];
            for (int j = 0; j < i; ++j) r1 += cnt2[j];
            ans = min(ans, r1);
            for (int j = i; j < 26; ++j) r2 += cnt2[j];
            for (int j = 0; j < i; ++j) r2 += cnt1[j];
            ans = min(ans, r2);
        }
        return ans;
    }
};
```