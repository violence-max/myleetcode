![image](/basicaldatastructure/binary_tree/image/image70.png)

解题过程：

没做出来，自底向上，一层一层除去叶子节点。

代码：

```cpp
class Solution {
public:
    int minIncrements(int n, vector<int>& cost) {
        int ans = 0;
        for (int i = n - 2; i > 0; i -= 2) {
            ans += abs(cost[i] - cost[i+1]);
            cost[i / 2] += max(cost[i], cost[i+1]);
        }
        return ans;
    } 
};
```