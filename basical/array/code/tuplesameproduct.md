题目描述：

![image](/basical/array/image/image111.png)

解决过程：

没做出来

- 哈希表统计不同的二元组合的乘积数目

代码：

```cpp
class Solution {
public:
    int tupleSameProduct(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int,int> map;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                map[nums[i] * nums[j]]++;
            }
        }
        int ans = 0;
        for (auto& [k,v] : map) {
            ans += v * (v - 1) * 4;
        }
        return ans;
    }
};
```