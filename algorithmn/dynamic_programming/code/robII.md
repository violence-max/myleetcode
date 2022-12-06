题目描述：  
![image](/algorithmn/dynamic_programming/image/image19.png)  
解决过程：  
靠，又没有写出来，一直在用之前做第一道题的想法去套路这个题目，考虑到达了最后一间房子的动态规划方程应该怎么去处理比较合适，没有想到啊，题解直接分别排除了第一间房子和最后一间的房子然后求了其中的最大值，离谱，暂时还没有想到反驳题解的理由，仔细想了一下，确实这样子处理可以得到偷盗的最大金额，强无敌！  
代码：  
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n =nums.size();
        if (n == 1) return nums[0];
        else if (n == 2) return max(nums[0],nums[1]);
        return max(robRange(nums,0,n-2),robRange(nums,1,n-1));
    }

    int robRange(const vector<int>& nums,int start,int end) {
        int first = nums[start],second = max(nums[start],nums[start+1]);
        int n = nums.size();
        for (int i = start + 2; i <= end; i++) {
            int temp = second;
            second = max(first+nums[i],second);
            first = temp;
        }
        return second;
    }
};
```