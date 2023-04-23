题目描述：  
![image](/algorithmn/dynamic_programming/image/image61.png)  
解题过程：  
没做出来  
- 动态规划：
1. dp[i]表示放置前i本书的最小高度
2. dp[i] = min(dp[j] + maxHeight), 0 ≤ j ≤ k < i
3. 需要注意我们放置书的顺序必须书在数组里的顺序一致，因此转移方程里的maxHeight是从后往前进行不断赋值的，我们在从后往前不断尝试找到dp[i]的最小值的过程中肯定会遇到某一层（实际上就是最后一层）的书的厚度超出了书架的最大厚度的情况，这时候我们只需要提前退出这种情况就好了  
代码：  
```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        vector<int> dp (n + 1, 1000000);
        dp[0] = 0;
        for (int i = 0; i < n; i++) {
            int maxHeight = 0, curWidth = 0;
            for (int j = i; j >= 0; j--) {
                curWidth += books[j][0];
                if (curWidth > shelfWidth) break;
                maxHeight = max(maxHeight, books[j][1]);
                dp[i + 1] = min(dp[i + 1], dp[j] + maxHeight);
            }
        }
        return dp[n];
    }
};
```