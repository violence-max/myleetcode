题目描述：

![image](/basical/array/image/image76.png)

解决过程：

30sac

- 排序+哈希表

代码：

```cpp
class Solution {
public:
    int distinctAverages(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        unordered_set<double> set;
        for (int i = 0, j = n - 1; i < j; i++, j--) {
            set.insert((nums[i] + nums[j]) / 2.0);
        }
        return set.size();
    }
};
```