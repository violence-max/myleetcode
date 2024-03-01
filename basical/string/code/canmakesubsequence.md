题目描述：

![image](/basical/string/image/image84.png)

解决过程：

第111场双周赛t2，ac

- 双指针

代码：

```cpp
class Solution {
public:
    bool canMakeSubsequence(string str1, string str2) {
        int l1 = str1.size(), l2 = str2.size();
        function<bool(int,int)> match = [&](int i, int j) -> bool {
            return str1[i] == str2[j] || (((str1[i] - 'a') + 1) % 26 == (str2[j] - 'a') % 26);
        };
        int ptr = 0;
        for (int i = 0; i < l1; ++i) {
            if (match(i, ptr)) {
                ++ptr;
            }
            if (ptr == l2) return true;
            if (l1 - i < l2 - ptr) break;
        }
        return false;
    }
};
```