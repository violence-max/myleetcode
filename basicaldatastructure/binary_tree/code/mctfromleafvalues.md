题目描述：

![image](/basicaldatastructure/binary_tree/image/image56.png)

解决过程：

不会做

- 区间dp
- 单调栈

代码：（区间dp→单调栈）

```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        // dp[i][j]表示arr数组下标区间[i,j]内能生成的二叉树的非叶子节点的最小和
        vector<vector<int>> dp (n, vector<int> (n, INT_MAX / 4));
        // mval[i][j]表示arr数组下标区间[i,j]内的最大值，用于计算非叶子节点的值
        vector<vector<int>> mval (n, vector<int> (n));

        for (int j = 0; j < n; j++) {
            mval[j][j] = arr[j];
            dp[j][j] = 0;
            for (int i = j - 1; i >= 0; i--) {
                // 计算dp[i][j]

                mval[i][j] = max(arr[i], mval[i + 1][j]);
                for (int k = i; k < j; k++) {
                    // 在区间[i,j]内k取[i,j)进行区间dp，右边是开区间因为dp[j][j]已经计算
                    dp[i][j] = min(dp[i][j], dp[i][k] /*左子树的非叶子节点的最小和*/  +
                                            dp[k+1][j] /*右子树的非叶子节点的最小和*/ + 
                                            mval[i][k] * mval[k + 1][j] /*左右子树的最大值的乘积*/ );
                }
            }
        }

        return dp[0][n-1];
    }
};
```

```bash
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        stack<int> stk;
        int res = 0;

        for (int x : arr) {
            while (!stk.empty() && stk.top() <= x) {
                int y = stk.top();
                stk.pop();
                // 问题转换成了不断合并arr相邻的两个数，合并出来的数是两个数的乘积，然后进而合并n-1个数的子问题
                if (stk.empty() || stk.top() > x) {
                    // 由于所求为最小和，因此当arr[i-1] > arr[i] && arr[i] <= arr[i+1]时，优先考虑将arr[i]和
                    // min(arr[i-1],arr[i+1])进行合并
                    res += y * x;
                } else {
                    res += stk.top() * y;
                }
            }
            stk.push(x);
        }
        while (stk.size() >= 2) {
            // 由于栈是严格递减的，因此从栈顶开始合并可以得到最小和
            int x = stk.top();
            stk.pop();
            res += x * stk.top();
        }
        return res;
    }
};
```