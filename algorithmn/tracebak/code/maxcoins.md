题目描述：  
![image](/algorithmn/tracebak/image/image25.png)  
解决过程：  
自己没想出来，看了题解  
为了更好地做出这道题目，题解把nums数组的首尾都赋值成了1，即把原来大小为n的数组改为大小为n+2的数组，然后把下标为0和下标为n+1的数据都赋值成了1  
- 回溯+记忆化搜索：
    1. 整体是分治的思想和流程逆序的思想
    2. 流程逆序：题目的描述中，每选择一个气球，这个气球的左右两个气球的相邻气球就会发生改变，因此选择一个气球相当于在数组中删除了一个元素。而流程逆序告诉了我们可以反过来思考：起初的数组中只有两个1，代表着resize之后的数组，然后我们每次选择一个气球加入到其中来使得按照题目逻辑计算出来得到的值最大
    3. 分治：循环遍历开区间(left,right)之间的气球，用sum代表选择这个气球后得到的值，然后在其左右两侧再分别递归计算
    4. 记忆化搜索：使用一个二维数组rec来记录计算好的值，例如：rec[i][j]
- 动态规划：**重点复习**
    1. 沿用了方法一的思路
    2. 从后往前进行遍历，每次循环的开始都保证区间里只存在一个元素，然后再增加  
    我个人觉得从后往前遍历只是为了更好地限制区间的右边界。因为如果从前往后，那么当左边界固定的时候，没有办法确定跳出循环的条件应该是什么，如果一开始区间里的有效元素的个数不是1个，那是没有办法进行状态转移的，因此每次求值都是需要需要被选中元素的两边的状态的  
代码：（回溯+记忆化搜索→动态规划）  
```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<int> _nums (n + 2);
        for (int i = 1; i <= n; i++) {
            _nums[i] = nums[i-1]; 
        }
        _nums[0] = _nums[n+1] = 1;
        vector<vector<int>> rec (n+2, vector<int> (n+2));
        return solve(rec,0,n+1,_nums);
    }

    int solve(vector<vector<int>>& rec, int left, int right,const vector<int>& nums) {
        if (left == right - 1) return 0;
        if (rec[left][right]) return rec[left][right];
        for (int i = left + 1; i < right; i++) {
            int sum = nums[left] * nums[i] * nums[right];
            sum += solve(rec,left,i,nums) + solve(rec,i,right,n);
            rec[left][right] = max(rec[left][right], sum);
        }
        return rec[left][right];
    }
};
```  
```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> rec(n + 2, vector<int>(n + 2));
        vector<int> val(n + 2);
        val[0] = val[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int k = i + 1; k < j; k++) {
                    int sum = val[i] * val[k] * val[j];
                    sum += rec[i][k] + rec[k][j];
                    rec[i][j] = max(rec[i][j], sum);
                }
            }
        }
        return rec[0][n + 1];
    }
};
```