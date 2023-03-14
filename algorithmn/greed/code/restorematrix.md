题目描述：  
![image](/algorithmn/greed/image/image20.png)  
解题过程：  
没做出来  
看了题解，关键是贪心的思路：  
- 每个格子能放的最大值是该行的和与该列的和的最小值，先把这个格子填满，该行与该列都减去这个被填下去的值，如果行的和或者列的和为0，剩下的值就补0，即跳过行或者列  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> restoreMatrix(vector<int>& rowSum, vector<int>& colSum) {
        int m = rowSum.size(), n = colSum.size();
        vector<vector<int>> ans (m, vector<int> (n));
        int i = 0, j = 0;
        while (i < m && j < n) {
            int val = min(rowSum[i],colSum[j]);
            ans[i][j] = val;
            rowSum[i] -= val;
            colSum[j] -= val;
            if (rowSum[i] == 0) i++;
            if (colSum[j] == 0) j++;
        }
        return ans;
    }
};
```