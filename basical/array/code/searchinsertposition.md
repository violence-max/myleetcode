题目描述：  
![image1](/basical/array/image/image2.png)
解题过程：
* 二分查找：在升序数组中找到第一个大于target目标值的索引i，i-1即是result。  
    * 具体的方法是在中间值小于target的时候排除左部分，大于等于target的时候记录答案之后排除右部分  
代码：  
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int length = nums.size();
        int left = 0;
        int right = length - 1;
        int mid;
        int result;
        while (left <= right){
            mid = (left + right) / 2;
            if (nums[mid] == target)
                break;
            else if(nums[mid] > target)
                right = mid - 1;
            else
                left = mid + 1;
        }
        result = (left <= right) ? mid : left;
        return result;
    }
};
```