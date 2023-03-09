题目描述：
![image3](/basical/array/image/image3.png)
解题过程：  
* 二分查找：
    1. 使用一个函数解决，用lower布尔变量决定是找target目标值在数组中的第一个位置或者最后一个位置：true->第一个位置（左边）；false->最后一个位置
    2. 把握第一个位置和最后一个位置的共性：第一个位置只需要在中间值大于等于target的时候不断记录答案，而最后一个位置需要在中间值大于target的时候不断记录答案，最后减一得到答案
    3. 在空数组中会得到leftidx = 0，rightidx = -1，因此计算出答案之后需要满足``leftidx <= rightidx``的条件，同时需要满足这两个索引位置的元素的值为target才说明找到了答案
```cpp
class Solution {
public:
    int binarySearch(vector<int>& nums,int target,bool lower) {
        int result = nums.size(),left = 0,right = nums.size() - 1;
        int mid;
        while (left <= right) {
            mid = ((right - left) >> 1) + left;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                result = mid;
            } else {
                left = mid + 1;
            }
        }
        return result;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums,target,true);
        int rightIdx = binarySearch(nums,target,false) - 1;
        if (leftIdx <= rightIdx && nums[leftIdx] == target && nums[rightIdx] == target)
            return vector<int> {leftIdx,rightIdx};
        return vector<int> {-1,-1};
    }
};
```