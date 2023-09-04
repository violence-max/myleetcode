代码：

![image](/basical/string/image/image85.png)

解决过程：
第112场双周赛t2，没做出来

- 脑筋急转弯

代码：

```
class Solution {
public:
    bool checkStrings(string s1, string s2) {
        int n = s1.size();
        string a1, a2, b1, b2;
        for (int i = 0; i < n; ++i) {
            if (i & 1) {
                a1 += s1[i];
                b1 += s2[i];
            } else {
                a2 += s1[i];
                b2 += s2[i];
            }
        }
        sort(a1.begin(), a1.end());
        sort(b1.begin(), b1.end());
        sort(b2.begin(), b2.end());
        sort(a2.begin(), a2.end());
        return a1 == b1 && a2 == b2;
        // return false;
    }
};
```