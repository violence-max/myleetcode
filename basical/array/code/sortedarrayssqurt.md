题目描述：  
![image](/basical/array/image/image10.png)
解题过程：  
思想过程很简单，直接找到正负数之间的分界线，然后把数组所有数据平方，最后像归并排序一样从中间往两头走  
官方题解有一个双指针，一头一尾直接比较的方法很nice  
**在写一个while循环的时候没有注意索引的大小，导致数组越界了**  
**code review**
现在看到这个题目第一反应是优先队列，按照数组中值的绝对值的大小从大到小加入将其索引加入优先队列中，然后直接出队将索引对应值加入答案数组中。但是突然意识到这样写比较复杂，需要自己写排序函数还有点不会写，而且这样子做的时间复杂度也是O(nlog(n)))，只能用双指针的方法了。 
自己一开始写的代码挺不错的呀，记录第一个非负数的索引，然后类归并排序合并两个序列。  
添加了题解的一种很妙的方法：一头一尾进行比较，把大的那个值逆序加入答案数组中，非常妙。  
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
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0, j = n - 1, pos = n - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                ans[pos] = nums[i] * nums[i];
                ++i;
            }
            else {
                ans[pos] = nums[j] * nums[j];
                --j;
            }
            --pos;
        }
        return ans;
    }
};
```