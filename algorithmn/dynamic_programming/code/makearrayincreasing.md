题目描述：  
![image](/algorithmn/dynamic_programming/image/image59.png)  
解题过程：  
没做出来  
- 动态规划：
1. 先对arr2进行排序并且去重复的操作
2. 设dp[i][j]表示arr1前i个元素经过j次替换之后使得arr1前i个元素保持严格递增的末位元素的最小值
3. 对于arr1的第i个元素，可以选择替换或者保留，因此转移方程为：
4. 对arr1进行循环，对于第i个元素而言，arr1的前i个元素组成的子序列与arr2最多执行min(i,arr2.size())次替换
5. 因此执行一个二重循环，j从0开始到min(i,arr2.size())结束
6. 需要注意的是每一次都必须保证dp[i][j]是arr1前i个元素组成的子序列里递增排序的最后一个元素的最小值：在arr2中查找严格大于dp[i-1][j-1]的最小元素（查找的时候可以直接从arr2.begin()+j-1开始，这就是技巧），然后dp[i][j]尝试更新最小值
7. 初始情况下，dp[i][j]全都设为inf；由于arr1的第一个元素需要进行比较判断，因此dp[0][0]赋值为-1，使得arr1[0]可以大于dp[0][0]
8. 由于我们需要寻找的使得arr1经过最小次数替换得到的递增数组，因此一旦我们遍历到了最后一个位置并且找到了一个可靠的替换次数，那么直接返回就好，这一定就是最少的替换次数  
代码：  
```cpp
constexpr int INF = 0x3f3f3f3f;

class Solution {
public:
    int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
        sort(arr2.begin(), arr2.end());
        arr2.erase(unique(arr2.begin(), arr2.end()), arr2.end());
        int n = arr1.size();
        int m = arr2.size();
        vector<vector<int>> dp(n + 1, vector<int>(min(m, n) + 1, INF));
        dp[0][0] = -1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= min(i, m); j++) {
                /* 如果当前元素大于序列的最后一个元素 */
                if (arr1[i - 1] > dp[i - 1][j]) {
                    dp[i][j] = arr1[i - 1];
                }
                if (j > 0 && dp[i - 1][j - 1] != INF) {
                    /* 查找严格大于 dp[i - 1][j - 1] 的最小元素 */
                    auto it = upper_bound(arr2.begin() + j - 1, arr2.end(), dp[i - 1][j - 1]);
                    if (it != arr2.end()) {
                        dp[i][j] = min(dp[i][j], *it);
                    }
                }
                if (i == n && dp[i][j] != INF) {
                    return j;
                }
            }
        }
        return -1;
    }
};
```