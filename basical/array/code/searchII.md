题目描述：  
![image](/basical/array/image/image35.png)  
解题过程：  
这道题和I有区别的地方就是这里可能会有重复数据，导致出现中间值和两头值相等的情况，这种情况只需要两头值向中间移进一格就好了，其余的和I一样。  
上面这句话是看题解的，我自己的写法是提前算好0位置上的数字重复到什么位置，然后强行判定mid到底位于哪个区间，但是巨慢无比这种方法。  
代码：  
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0,right = n - 1;
        int i = 1;
        while (i < n && nums[i] == nums[0])
            i++;
        int edge = i;
        while (left <= right) {
            int mid = (right + left) >> 1;
            if (nums[mid] == target) return true;
            if (nums[0] < nums[mid] || (0 <= mid && mid < edge)) {
                if (nums[0] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n-1]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return false;
    }
};
```