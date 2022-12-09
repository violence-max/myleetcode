题目描述：  
![image](/basicaldatastructure/stackandquere/image/image14.png)  
这道题是真的好玩，直接看题解，因为脑子过载了想不出来  
动态规划：两遍扫描得到左右最大值，然后一遍扫描拿到每个位置降雨量，贼寂寞  
单调栈：保证栈是递减的，不严格，栈中至少存在两个元素的时候才考虑中间哪个元素的位置可以接的雨水，用遍历到的值和栈顶的值作比较，若大于，则弹出栈顶，此时栈为空则退出循环继续遍历下一个值，否则再拿到栈顶元素，然后用弹出的值的两边的最小最大高度减去弹出的高度  
双指针：最无敌，直接从左向右从右向左分别移动最高矩形，差值即为填充的雨水  
代码：  
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        int left = 0,right = n - 1,leftMax = 0,rightMax = 0;
        int ans = 0;
        while (left < right) {
            leftMax = max(leftMax,height[left]);
            rightMax = max(rightMax,height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left++];
            } else {
                ans += rightMax - height[right--];
            }
        }
        return ans;
    }
};
```