题目描述：  
![image](/algorithmn/greed/image/image5.png)  
解题过程：  
花了五分钟思考思路，没思考明白，但是思考了一个大概，就是每次更新能跳到最远的位置，如果最远的位置发生一次改变则跳的次数加一，直到跳到最远的位置为止。  
题解很好地实现了我的这个思路，从左往右遍历，直到值到达边界时次数才加一，而且边界才和最远边界对齐，并且再最后一个位置之前的一个位置就退出循环了。  
强无敌！！  
代码：  
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int count = 0,rightMost = 0,side = 0;
        for (int i = 0; i < nums.size() - 1; i++) {
            if (i <= rightMost) {
                rightMost = max(rightMost,i+nums[i]);
                if (i == side) {
                    side = rightMost;
                    count++;
                }
            }
        }
        return count;
    }
};
```