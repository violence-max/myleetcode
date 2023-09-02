题目描述：

![image](/basical/array/image/image103.png)

解决过程：
3分钟ac

题解给出了一种只扫描一遍的做法，记录一下

代码：

```
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int ans = 0, n = forts.size();
        for (int i = 0; i < n; ++i) {
            if (forts[i] == 1) {
                int j = i + 1;
                while (j < n && forts[j] == 0) ++j;
                if (j < n && forts[j] == -1) ans = max(ans, j - i - 1);
                int k = i - 1;
                while (k >= 0 && forts[k] == 0) --k;
                if (k >= 0 && forts[k] == -1) ans = max(ans, i - k - 1);
            }
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int ans = 0, n = forts.size(), pre = -1;
        for (int i = 0; i < n; ++i) {
            if (forts[i] == 1 || forts[i] == -1) {
                if (pre >= 0 && forts[pre] != forts[i]) {
                    ans = max(ans, i - pre - 1);
                }
                pre = i;
            }
        }
        return ans;
    }
};
```