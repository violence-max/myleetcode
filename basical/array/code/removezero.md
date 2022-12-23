题目描述：  
![image](/basical/array/image/image9.png)
解题过程：  
很典型的一个不使用额外空间复杂度移动一个数组里的元素的题目，常规来讲，我们会想到的方法应该是使用双指针，一个移动数组，一个在已经排好序的数组的末尾，移动数组的指针一旦检测到非零元素则进行交换。我采用的方法是和之前移动元素一样的，先检测0是否存在的那个思路。  
代码：  
**code review**
看了以前自己写的代码发觉写得实在是太冗长了，于是修改了一下，主要修改的思路是从一开始就设置双指针，右指针不断往后移动，左指针始终停留在需要交换的位置上（0串第第一个位置）。基本算法逻辑是：在右指针到达数组结尾之前不断循环，如果右指针不为0就让左指针找到右指针左侧的0串的第一个位置并发生交换，然后左指针右移。每次循环右指针都需要右移。这样写比较简洁。  
看了题解，题解是用左指针维护不为0的数组的末尾，右指针进行探测，右指针探测到非0值是就进行交换然后两个指针向右移动。感觉这个方法发生了很多无用交换，不是很可取。  
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