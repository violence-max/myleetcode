题目描述：  
![image](/basical/array/image/image43.png)  
解题过程：  
二分查找，十分钟自己撸出来了，不过题解直接拿中点值和右边的值进行比较更加好  
代码：（我的→题解）  
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while (1) {
            int mid = (left + right) >> 1;
            if (nums[left] <= nums[mid] && nums[mid] <= nums[right]) {
                return nums[left];
            } else if (nums[left] <= nums[mid]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
    }
};
```
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (nums[pivot] < nums[high]) {
                high = pivot;
            }
            else {
                low = pivot + 1;
            }
        }
        return nums[low];
    }
};
```