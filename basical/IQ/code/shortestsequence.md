题目描述：

![image](/basical/IQ/image/image56.png)

解决过程：

没做出来

- 构造题

代码：

```cpp
class Solution {
public:
    int shortestSequence(vector<int>& rolls, int k) {
        int n = rolls.size();
        int vis[k+1];
        memset(vis, 0, sizeof vis);
        int ans = 1; // 答案最少是1
        int count = 0;
        for (int roll : rolls) {
            if (vis[roll] < ans) {
                vis[roll]++;
                count++;
                if (count == k) {
                    ans++;
                    count = 0;
                }
            }
        }
        return ans;
    }
};
```