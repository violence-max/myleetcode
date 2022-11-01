题目描述：  
![image](/basicaldatastructure/array/image/image8.png)
解题思路：  
设置一个双指针，一个指针指向下一个应该赋值的位置，一个指针对数组进行扫描，如果扫描到和已经被赋值了的最后一个值不相等的值，则进行赋值，并让第一个指针向右移动  
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (!nums.size()) return 0;
        int origin = nums[0] , i = 1 ,j = 1 ,length = nums.size();
        while (i < length) {
            if (nums[i] != origin) {
                nums[j] = nums[i];
                origin = nums[j++];
            }
            i++;
        }
        return j;
    }
};
```