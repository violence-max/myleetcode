题目描述：

![image](/basical/string/image/image75.png)

解决过程：

第107场双周赛，23分钟，错一次，没想好直接超时了

代码：

```cpp
class Solution {
public:
    int longestSemiRepetitiveSubstring(string s) {
        int ptr = -1;
        int cnt = 0;
        int length = 1;
        int n = s.size();
        int i = 0;
        while (i < n) {
            int j = i;
            while (j < n - 1 && (s[j] != s[j + 1] || cnt < 1)) {
                if (s[j] == s[j + 1]) {
                    cnt = 1;
                    ptr = j + 1;
                }
                j++;
            }
            length = max(length, j - i + 1);
            if (j == n - 1) {
                break;
            }
            // cout << i << ' ' << j << ' ' << length << ' ' << cnt << ' ' << ptr << endl;
            i = ptr == -1 ? j + 1 : ptr;
            if (i == n - 1) {
                break;
            }
            cnt = 0;
        }
        return length;
    }
};
```