题目描述：

![image](/basicaldatastructure/stackandquere/image/image31.png)

解决过程：

没做出来

- 基于优先队列的贪心

代码：

```cpp
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        priority_queue<int, vector<int>, greater<int>> q;
        int n = heights.size();
        int sum = 0;
        for (int i = 1; i < n; ++i) {
            int deltaH = heights[i] -heights[i-1];
            if (deltaH > 0) {
                q.push(deltaH);
                if (q.size() > ladders) {
                    sum += q.top();
                    q.pop();
                }
                if (sum > bricks) {
                    return i - 1;
                }
            }
        }
        return n - 1;
    }
};
```