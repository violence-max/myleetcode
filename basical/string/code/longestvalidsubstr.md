题目描述：

![image](/basical/string/image/image82.png)

解决过程：

第354场周赛t4，没做出来，后来发现其实滑窗暴力是可以做的。。。只要从新加入的字符开始检测就可以了，因为forbidden中每个字符串的长度都不长

代码：

```cpp
class Solution {
public:
    int longestValidSubstring(string word, vector<string>& forbidden) {
        int n = word.size();
        unordered_set<string> s (forbidden.begin(),forbidden.end());
        int ans = 0;
        int left = 0;
        for (int i = 0; i < n; ++i) {
            int x = i;
            while (x >= left && i - x + 1 <= 10) {
                if (s.count(word.substr(x, i - x + 1))) {
                    left = x + 1;
                    break;
                }
                x--;
            }
            ans = max(ans, i - left + 1);
        }
        return ans;
    }
};
```