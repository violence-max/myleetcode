题目描述：  
![image](/basical/array/image/image16.png)  
解题过程：  
这个题之前做过，看到这个题第一个想法就是先排序，然后用一个双指针一前一后进行值的比较，算下来就是一个时间复杂度为O(n^2)的题目，但是对于双指针移动的细节我记得不是很清楚了，尤其是这个题的一大非常重要的要求：不能有重复的元组。我在第一次做的时候应该是用跳过相同元素的方法排除了重复的元素，而且思想相当简单，就是和用三数之和和0做比较，如果大于则右指针建议，如果小于则做指针加一，但是不是简单的减一和加一，而是要跳过所有相同元素再加一和减一。因为我不想把之前的代码又重新敲一遍，我就看了一下题解，题解的方法也和我之前的解法差不多，就是双指针的处理不太一样，题解的方法只移动右指针，然后用左右指针是否相等这个判断条件进行提前跳出，这样看来还是题解的方法好一点，因为没有重复的扫描。  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> result;
        int length = nums.size();
        for (int i = 0; i < length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int right = length - 1;
            for (int left = i + 1; left < length - 1; left++) {
                if (left > i + 1 && nums[left] == nums[left - 1]) {
                    continue;
                }
                int sum = nums[i] + nums[left] + nums[right];
                while (left < right && sum > 0) {
                    sum = nums[i] + nums[left] + nums[--right];                    
                }
                if (right == left) break;
                else if (sum == 0) {
                    result.push_back({nums[i],nums[left],nums[right]});
                }
            }
        }
        return result;
    }
};
```