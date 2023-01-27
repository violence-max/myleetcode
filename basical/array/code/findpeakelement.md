题目描述：  
![image](/basical/array/image/image45.png)  
解决过程：  
要用二分法来解决这种问题我还真不太会，于是看了题解。发现自己在审题的时候忽略了一个很重要的元素——由于题目保证相邻的两个元素绝对不相同，而且可以把nums[-1]和nums[n]看作是负无穷，因此峰值是绝对存在的，题解基于此给出了三种解法：  
1. 直接返回最大值
2. 爬坡法
3. 基于爬坡法的二分  
只有方法3是符合题目要求的。在code友对题解的改进下，省去了很多不必要的地方，主要体现在于：只需要判断中点值和中点值右边的值的大小关系而无序考虑中点值是否为最有节点，因为循环条件保证了当前寻找的区间一定是至少有两个元素值的。因此就可以一直压缩寻找，最终退出循环时无论是哪种情况都可以保证左边边界的元素是峰值  
代码：  
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        while (low < high) {
            int mid=  (low + high) >> 1;
            if (nums[mid] < nums[mid+1]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
};
```