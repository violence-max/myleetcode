题目描述：  
![image](/basical/array/image/image21.png)  
解决过程：  
可能是动态规划入魔了，想不出来还一直死磕，这道题根本跟子问题没什么关系，所以还是想想别的方法吧（像以前那样）  
双指针的解法太寂寞了，一个双指针完事  
代码： 
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int m = height.size();
        int left = 0,right = m - 1;
        int ans = 0;
        while (left < right) {
            ans = max(ans,(right-left)*min(height[left],height[right]));
            if (height[left] <= height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return ans;
    }
};
```