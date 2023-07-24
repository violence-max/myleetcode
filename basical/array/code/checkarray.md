题目描述：

![image](/basical/array/image/image97.png)

解决过程：

第353场周赛t4，没做出来

- 差分

代码：

```
class Solution {
public:
    int sum[100020];
    bool checkArray(vector<int>& nums, int k) {
        if (k == 1) return true;
        int n = nums.size();
        int last = 0;
        memset(sum, 0, sizeof sum);
        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            if (i >= k) last -= sum[i-k];
            if (x < last) return false;
            if (n - i < k && x != last) return false;
            sum[i] = x - last;
            last += sum[i];
        }
        return true;
    }
};
```