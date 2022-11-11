题目描述：  
![image](/basical/array/image/image17.png)  
解题过程：  
一开始看到这个题我是无语的，因为这个跟三数之和几乎没有任何区别，就是多增加了一个target还有变成了四数，我觉得时间复杂度又要上升一个层级了，我就很无语，然后就看了题解，但是没看思路，我直接去看时间复杂度，竟然真的是O(n^3)，我就只能硬着头皮做了。就拿三数之和的方法去做，然后跑了一下，发现sum值越界了。看来我对如何处理越界还不是特别清楚，要么所有值一开始都设置成long型，要么就在做和时把第一个值强转为long型。这个题其实可以做得更巧妙一点，就是多跳过一些不必要的扫描，比如如果排序之后前面四个值之和都大于target了就没必要进行接下来的扫描了。  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int length = nums.size();
        vector<vector<int>> result;
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int left = i + 1; left < length - 2; left++) {
                if (left > i + 1 && nums[left] == nums[left - 1]) continue;
                int right = length - 1;
                for (int left2 = left + 1; left2 < length - 1; left2++) {
                    if (left2 > left + 1 && nums[left2] == nums[left2 - 1]) continue;
                    long sum = (long)nums[i] + nums[left] + nums[left2] + nums[right];
                    while (left2 < right && sum > target) {
                        sum = (long)nums[i] + nums[left] + nums[left2] + nums[--right];
                    }
                    if (right == left2) break;
                    else if (sum == target) {
                        result.push_back({nums[i],nums[left],nums[left2],nums[right]});
                    }
                }
            }
        }
        return result;
    }
};
```