题目描述：  
![image](/algorithmn/dynamic_programming/image/image25.png)  
解决过程：  
靠，一直在思考O(n)的时间复杂度，没有想到如果换成更高一点的时间复杂度其实是有机会的。我在思考的过程中几乎已经把动态规划方程写出来了，就是等于前面所有的子序列长度的最大值加上当前，但是对于状态的定义没有定义好，只定义了dp[i]是宝库哦第i+1个字符在内的最长递增子序列的长度，并没有考率到要不要以当前数字结尾。看了题解，没想到还可以这样定义动态规划方程。  
代码：  
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n+1);
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i],dp[j]+1);
                }
            }
        }
        return *max_element(dp.begin(),dp.end());
    }
};
```  
进阶：贪心+二分查找  
强无敌的一个方法，直接把时间复杂度降到了O(nlog(n))，md，每次拿增加的幅度都尽量小一点，这种方法是真的离谱，自己实现的时候也除了一些bug，主要是在判断条件上面没有弄清楚思路。其实也弄清楚了，就是实现的时候手贱了，毕竟自顶向下是一个非常困难的事情。  
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size(),len = 1;
        vector<int> dp(n+1);
        dp[1] = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] > dp[len]){
                dp[++len] = nums[i];
            } else {
                int left = 1,right = len,pos = 0;
                while (left <= right) {
                    int mid = (left + right) >> 1;
                    if (dp[mid] < nums[i]) {
                        pos = mid;
                        left = mid + 1;
                    } else {
                        right = mid - 1;
                    }
                }
                dp[pos+1] = nums[i];
            }
        }
        return len;
    }
};
```