题目描述：

![image](/basical/string/image/image80.png)

解决过程：

没做出来，并且读错题目，没注意可以重新排列子串

- 异或前缀和统计字母奇偶性

代码：

```cpp
class Solution {
public:
    vector<bool> canMakePaliQueries(string s, vector<vector<int>>& queries) {
        int n = s.size();
        vector<int> count (n + 1);
        for (int i = 0; i < n; ++i) {
            // 使用异或前缀和数组计算字母的奇偶性

            count[i + 1] = count[i] ^ (1 << (s[i] - 'a'));
        }

        vector<bool> res;
        for (const auto& query : queries) {
            int l = query[0], r = query[1], k = query[2];
            int bits = 0, x = count[r + 1] ^ count[l]; // 获取[l,r]区间字母的奇偶性
            while (x) {
                // 计算x中1的个数，即[l,r]区间的出现次数为奇数的字母的个数
                x = x & (x - 1);
                bits++;
            }
            // 出现次数为偶数的字母排到两边，最后会剩下bits个不能构成回文串的字母
            // 如果x / 2 <= k，则剩下的一半可以构成回文串
            // cout << l << ' ' << r << ' ' << x << ' ' << bits << endl;
            res.push_back(bits / 2 <= k);
        }
        return res;
    }
};
```