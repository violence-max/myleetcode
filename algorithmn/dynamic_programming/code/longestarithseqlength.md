题目描述：  
![image](/algorithmn/dynamic_programming/image/image60.png)  
解决过程：  
没做出来（还想用排序做来着，结果是肯定不行）  
- 动态规划：  
代码：  
```cpp
class Solution {
public:
    int longestArithSeqLength(vector<int>& nums) {
        auto [minn, maxx] = minmax_element(nums.begin(), nums.end());
        int diff = *maxx - *minn;
        int ans = 2;
        for (int d = -diff; d <= diff; d++) {
            // 对所有可能的公差进行循环
            vector<int> f (*maxx + 1, - 1); // f[num]表示最后一个值为num且公差为d的最长子序列的长度，-1表示占位符
            for (int num : nums) {
                if (int prev = num - d; prev >= *minn && prev <= *maxx && f[prev] != -1) {
                    f[num] = max(f[num], f[prev] + 1);
                    ans = max(ans, f[num]);
                }
                f[num] = max(f[num], 1); // f[num]第二种可能的转移方式：值至少为1
            }
        }
        return ans;
    }
};
```