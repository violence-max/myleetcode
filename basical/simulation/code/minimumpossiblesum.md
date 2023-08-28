题目描述：

![image](/basical/simulation/image/image14.png)

解决过程：

第360场周赛t2，没做出来

按照题意构造就好了

代码：

```cpp
class Solution {
public:
    int used[300020];
    long long minimumPossibleSum(int n, int target) {
        int now = 1, idx = 0;
        long long ans = 0;
        memset(used, 0, sizeof used);
        while (idx < n) {
            if (now < target) {
                if (!used[target - now]) {
                    used[now] = true;
                    ans += now;
                    idx++;
                }
            } else {
                ans += now;
                idx++;
            }
            ++now;
        }
        return ans;
    }
};
```