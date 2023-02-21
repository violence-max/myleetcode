题目描述：  
![image](/algorithmn/dynamic_programming/image/image49.png)  
解决过程：  
没做出来，看了题解，没想到可以用动态规划。最关键的部分应该在于定义dp[i]，题解定义的是0到i区间需要打开的最小的水龙头的个数，我觉得很妙，定义了这个之后，排序+区间内的判断就ok了  
题解的贪心方法也很有趣，跟跳跃游戏类似，主要是维护了每一个点能到达的最远距离  
代码:（动态规划→贪心）  
```cpp
class Solution {
public:
    int minTaps(int n, vector<int>& ranges) {
        vector<pair<int,int>> intervals (n + 1);
        for (int i = 0; i <= n; i++) {
            int start = max(0,i - ranges[i]);
            int end = min(n,i + ranges[i]);
            intervals.emplace_back(make_pair(start,end));
        }
        sort(intervals.begin(),intervals.end());
        vector<int> dp (n + 1, INT_MAX);
        dp[0] = 0;
        for (auto [start,end] : intervals) {
            if (dp[start] == INT_MAX) return -1;
            for (int j = start + 1; j <= end && j <= n; j++) {
                dp[j] = min(dp[j],dp[start] + 1);
            }
        }
        return dp[n];
    }
};
```  
```cpp
class Solution {
public:
    int minTaps(int n, vector<int>& ranges) {
        vector<int> rightmost (n + 1);
        for (int i = 0; i <= n; i++) {
            int start = max(0,i - ranges[i]);
            int end = min(n,i + ranges[i]);
            rightmost[start] = max(rightmost[start],end);
        }
        int ret = 0, last = 0, pre = 0;
        for (int i = 0; i < n; i++) {
            last = max(last,rightmost[i]);
            if (i == last) {
                return -1;
            }
            if (i == pre) {
                ret++;
                pre = last;
            }
        }
        return ret;
    }
};
```