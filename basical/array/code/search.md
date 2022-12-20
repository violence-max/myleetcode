题目描述：  
![image](/basical/array/image/image25.png)    
解题过程：  
后知后觉发现要用二分法，一开始居然还想着寻找是在哪里发生的旋转然后再用二分法之类的，相当于两次使用二分法，很显然这样子的想法是错误的。  
看了答案，居然可以用局部的二分法，因为旋转数组无论如何都有一边是有序的，所以可以根据中间值和第一个值的大小来判断从0到中间值的这段区间是否为升序的部分，然后就是常规的根据大小的比较结果进行区间的细化  
代码：  
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0,right = n - 1;
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (nums[mid] == target) return mid;
            if (nums[0] <= nums[mid]) {
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
        return -1;
    }
};
```