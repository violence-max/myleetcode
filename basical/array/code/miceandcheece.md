题目描述：

![image](/basical/array/image/image78.png)

解决过程：

没做出来，但是code友都说很简单

- 贪心+优先队列

代码：

```cpp
class Solution {
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) {
        int n = reward1.size();
        int ans = 0;
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int i = 0; i < n; i++) {
            ans += reward2[i];
            pq.emplace(reward1[i] - reward2[i]);
            if (pq.size() > k) {
                // 保留最大的k个差值
                pq.pop();
            }
        }
        while (pq.size() > 0) {
            ans += pq.top();
            pq.pop();
        }
        return ans;
    }
};
```