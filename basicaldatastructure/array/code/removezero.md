题目描述：  
![image](/basicaldatastructure/array/image/image9.png)
解题过程：  
很典型的一个不使用额外空间复杂度移动一个数组里的元素的题目，常规来讲，我们会想到的方法应该是使用双指针，一个移动数组，一个在已经排好序的数组的末尾，移动数组的指针一旦检测到非零元素则进行交换。我采用的方法是和之前移动元素一样的，先检测0是否存在的那个思路。  
代码：  
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i;
        int length = (int)nums.size();
        bool exitFlag = false;

        for (int k = 0; k < length; k++) {
            if (nums[k] == 0) {
                exitFlag = true;
                i = k;
                break;
            }
        }

        if (!exitFlag) return;

        for (int q = i + 1; q < length; q++) {
            if (nums[q] != 0) {
                swap(nums[i++],nums[q]);
            }
        }
    }
};
```