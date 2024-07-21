题目：

![image](/basical/array/image/image118.png)

解决：

没做出来，大半年以来的康复训练

动态规划：

```sql

class Solution {
public:
    int maximumSum(vector<int>& arr) {
        int n = arr.size();
        int dp0 = arr[0], dp1 = 0;
        int res = dp0;
        for (int i = 1; i < n; ++i) {
            dp1 = max(dp0, dp1 + arr[i]);
            dp0 = max(dp0, 0) + arr[i];
            res = max(res, max(dp0, dp1));
        }
        return res;
    }
};
```