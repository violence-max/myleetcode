题目描述：

![image](/basical/array/image/image84.png)

解题过程：
第351场周赛t3，因为t2卡得很久，这题1500分左右的题反倒没有做出来

代码：

```cpp
class Solution {
public:
    int numberOfGoodSubarraySplits(vector<int>& nums) {
        vector<int> locate;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 1) {
                locate.push_back(i);
            }
        }
        if (locate.empty()) {
            return 0;
        }
        long long ans = 1;
        const int mod = 1e9 + 7;
        int ll = locate.size();
        for (int i = 1; i < ll; ++i) {
            int distance = locate[i] - locate[i - 1];
            ans = (ans * (long long)distance) % mod;
        }
        return ans;
    }
};
```