题目描述：  
![image1](/basical/array/image/image2.png)
解题过程：还是使用二分法，只不过多了一个搜索不到对应值的时候要返回应该插入的位置罢了，结尾加一个判断条件，如果搜索得到就返回索引，搜索不到就返回`left`值，因为此时left值是加1后大于right值的，刚好符合插入位置  
**位运算应该比除法更快**，所以对于mid的求值可以这么写：`mid = ((right - left) >> 1) + left;`   
**code review**
这里的写法感觉不够抽象，还是一个比较稚嫩的二分查找写法。通过判断中位数是否和目标值相等进行返回，找不到则说明来到最大的比目标值要小的数据，这时左指针已经来到了正确的位置，直接返回即可。  
但是我要说的是，可以直接在中间值大于等于目标值的时候记录答案，此时记录的下标是第一个比目标值要大或者与之相等的值得下标，即答案  
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