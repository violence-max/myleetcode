题目描述：  
![image](/basical/array/image/image21.png)  
解决过程：  
可能是动态规划入魔了，想不出来还一直死磕，这道题根本跟子问题没什么关系，所以还是想想别的方法吧（像以前那样）  
* 双指针：直观地观察如果要使得接水的容器的容积最大，那么我们希望它的宽和高都非常大。而对于数组中任意两个元素而言，宽为两个元素的下标之差（大减小），高位两个元素的高度的最小值
1. 用left指针指向数组第一个元素，right指针指向数组最后一个元素
2. 每次计算对应的[left,right]区间内的容器的体积，满足条件则更新最大值
3. 高度较小的一边往中间移动，left和right相遇时推出while循环
代码： （双指针）
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