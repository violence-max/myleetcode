题目描述：  
![image1](/basicaldatastructure/array/image/image2.png)
解题过程：还是使用二分法，只不过多了一个搜索不到对应值的时候要返回应该插入的位置罢了，结尾加一个判断条件，如果搜索得到就返回索引，搜索不到就返回`left`值，因为此时left值是加1后大于right值的，刚好符合插入位置  
**位运算应该比除法更快**，所以对于mid的求值可以这么写：`mid = ((right - left) >> 1) + left;`   
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