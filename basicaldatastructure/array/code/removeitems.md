题目描述：  
![image](/basicaldatastructure/array/code/removeitems.md)
解题过程：类似于快速排序的操作，先扫描一部分数据看是否存在val，如果不存在则直接返回数组长度，如果存在则记录位置，从下一个位置开始扫描，如果不是val则进行交换，并且记录的位置加一，最后返回记录的位置，即长度。  
代码：  
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