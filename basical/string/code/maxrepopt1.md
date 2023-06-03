题目描述：

![image](/basical/string/image/image72.png)

解决过程：

没做出来

- 滑动窗口

代码：（滑动窗口）

```cpp
class Solution {
public:
    int maxRepOpt1(string text) {
        unordered_map<char, int> map;
        for (auto c : text) {
            // 计算text中每个字符出现的次数
            map[c]++;
        }

        int res = 0, n = text.size();
        for (int i = 0; i < n; ) {
            int j = i;
            // 寻找一段连续的字符相同的子串
            while (j < n && text[j] == text[i]) {
                j++;
            }

            // 如果这段连续的字符相同的子串前后有可以交换的空间，
            // 并且总个数小于字符text[i]的总数，则更新答案
            if (j - i < map[text[i]] && (i > 0 || j < n)) {
                res = max(res, j - i);
            }

            int k = j + 1;
            // 考虑j之后可能有连续的text[i]字符，并且每次替换总是优先
            // 替换位置j的字符（如果不能替换说明到头了，k也无需考虑），
            // 则这两段子串拼接起来可以得到更长的答案。

            // 同时需要考虑aaaaabaaaaa这种情况，此时k - i大于map[a]，因此
            // 在更新答案时应取k - imap[a]的最小值
            while (k < n && text[k] == text[i]) {
                k++;
            }
            res = max(res, min(k - i, map[text[i]]));

            i = j;
        }

        return res;
    }
};
```