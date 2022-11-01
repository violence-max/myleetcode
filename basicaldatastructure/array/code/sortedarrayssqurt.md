题目描述：  
![image](/basicaldatastructure/array/image/image10.png)
解题过程：  
思想过程很简单，直接找到正负数之间的分界线，然后把数组所有数据平方，最后像归并排序一样从中间往两头走  
官方题解有一个双指针，一头一尾直接比较的方法很nice  
**在写一个while循环的时候没有注意索引的大小，导致数组越界了**  
代码：  
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int length = (int)nums.size();
        vector<int> result;
        int index = 0,count = 0;
        result.resize((int)length,0);
        while (index < length && nums[index] < 0)
            index++;
        for (int i = 0; i < length; i++) 
            nums[i] *= nums[i];
        int ascendIndex = index,descendIndex = index - 1;
        while (ascendIndex < length && descendIndex >= 0) {
            result[count++] = (nums[ascendIndex] <= nums[descendIndex]) ? nums[ascendIndex++] : nums[descendIndex--];
        }
        while (ascendIndex < length) 
            result[count++] = nums[ascendIndex++];
        while (descendIndex >= 0)
            result[count++] = nums[descendIndex--];
        return result;
    }
};
```