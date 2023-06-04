题目描述：

![image](/basical/array/code/semiorderpermutation.md)

解题过程：

竞赛题，ac

代码：

```cpp
class Solution {
public:
    int semiOrderedPermutation(vector<int>& nums) {
        int n = nums.size();
        int pos1 = -1, pos2 = -1;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) {
                pos1 = i;
            } else if (nums[i] == n) {
                pos2 = i;
            }
        }
        
        int res = pos1 + (n - 1 - pos2);
        res -= (pos1 < pos2) ? 0 : 1;
        return res;       
    }
};
```