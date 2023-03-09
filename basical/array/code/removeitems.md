题目描述：  
![image](/basical/array/image/image7.png)  
解题过程：类似于快速排序的操作，先扫描一部分数据看是否存在val，如果不存在则直接返回数组长度，如果存在则记录位置，从下一个位置开始扫描，如果不是val则进行交换，并且记录的位置加一，最后返回记录的位置，即长度。  
代码：  
**code review**
自己写的方法类似于一种简单的模拟，先找到第一个目标值的位置，燃火往后找到第一个不是目标值的下标，交换两个指针的数字，然后两个指针同时往后移动，但是这样写代码连比较大，于是出现了优化算法：
1. 左指针赋初值为0，右指针从0开始遍历至数组结尾，如果右指针所指向的值不是目标值，则将右指针所指向的位置的值赋值给左指针所指向的位置的值，这样就可以通过移动左右指针来得到一个不含目标值的数组
2. 更加优化：左值针赋初值为0，右指针赋初值为数组的最后一个位置，使用while循环，当左右指针相遇时退出循环，如果左值针位置的值为目标值则于右指针位置的值进行交换并且相左移动右指针，否则向右移动左指针，这样相比于方法一最好的情况下少遍历了一次数组（方法1需要两个指针同时移动）。  
* 双指针：左指针维护已经处理好的数组的下一个位置，右指针遍历到不是目标值的时候左右指针的元素进行交换
```cpp
class Solution {
public:
    void swap(int &a,int &b) {
        int temp = a;
        a = b;
        b = temp;
    }

    int removeElement(vector<int>& nums, int val) {
        int i = -1,length = (int)nums.size();
        bool exitFlag = false;

        for (int j = 0; j < length; j++) {
            if (nums[j] == val) {
                exitFlag = true;
                i = j;
                break;
            }
        }

        if (!exitFlag) return length;
        for (int k = i + 1; k < length; k++) {
            if (nums[k] != val && k > i) {
                swap(nums[k],nums[i++]);
            }
        }
        
        return i;
    }
};
```