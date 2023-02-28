题目描述：  
![image](/basical/array/image/image33.png)  
解题过程：  
这道题真的是把我秀到头皮发麻。  
我一直以为是一道模拟题，只要对数组的左右两边的某个数进行取舍就可以了，而且我居然还真的把数组的某些值进行删除操作。  
看了题解，是一道滑动窗口的经典题目了。  
算法流程：  
1. 起初设置左前缀和lsum为0，左指针为-1，右后缀和为整个数组的值之和，右指针为0
2. 左指针不断向右移动，若左指针的值不为-1，则让左前缀和加上做指针的值；在右指针没有到达数组的末尾以及左前缀和与右后缀和加起来比x大时，不断减去右指针位置的值，此时右后缀的和不断减小
3. 当lsum和rsum之和与x相等时，记录最小操作数
代码：  
```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        int sum = accumulate(nums.begin(),nums.end(),0);
        int n = nums.size();
        if (sum < x)
            return -1;
        int lsum = 0,left,right = 0,rsum = sum;
        int ans = n + 1;
        for (int left = -1; left < n; ++left) {
            if (left != -1) {
                lsum += nums[left];
            }
            while (right < n && lsum + rsum > x) {
                rsum -= nums[right];
                ++right;
            }
            if (lsum + rsum == x) {
                int length = left + 1 + n - right;
                ans = ans > length ? length : ans;
            }
        }
        return ans > n ? -1 : ans;
    }
};
```