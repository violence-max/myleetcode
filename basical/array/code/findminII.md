题目描述：  
![image](/basical/array/image/image44.png)  
解题过程：  
- 亮点：旋转数组如果含有重复元素，那么对于二分查找或者查询极值而言，与不含重复值相比最大的不同点在于中间值可能等于两头值，这个时候采取的操作是右端点往中间移一步，因为不管中间点是在左半部分还是右半部分还是在升序的部分，最右的端点始终有中间点作为替代，因此无序担心最右端点为最小值的情况  
自己发现了归并查找最小值，不过好像比较慢，还是二分比较好  
代码：（归并→二分）  
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        return findMin(nums,0,nums.size()-1);
    }

    int findMin(vector<int>& nums,int left,int right) {
        if (right - left + 1 <= 2) {
            return min(nums[left],nums[right]);
        } else {
            int mid = (left + right) >> 1;
            return min(findMin(nums,left,mid),findMin(nums,mid+1,right));
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
            else if (nums[pivot] > nums[high]) {
                low = pivot + 1;
            }
            else {
                high -= 1;
            }
        }
        return nums[low];
    }
};
```