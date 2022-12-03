题目描述：  
![image](/algorithmn/dynamic_programming/image/image4.png)  
解题过程： 
跟斐波那契数列几乎一样，就是动态规划转移方程是取最小值而已，忽略了最后要登上楼梯，我还以为是爬到最后一个节点就可以，改了一次就过了。 
代码： 
```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        if (n == 1) return cost[0];
        else if (n == 2) return min(cost[0],cost[1]);
        else {
            int first = 0,second = 0;
            int ans;
            for (int i = 2; i < n; i++) {
                ans = min(first+cost[i-2],second+cost[i-1]);
                first = second;
                second = ans;
            }
            return min(first+cost[n-2],second+cost[n-1]);
        }
    }
};
```