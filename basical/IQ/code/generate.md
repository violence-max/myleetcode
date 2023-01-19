题目描述：  
![image](/basical/IQ/image/image16.png)  
解题过程：  
一开始用递归忘记记忆化了，后来加上就0ms了。我发现这题用迭代来写更简洁，不必自顶向下  
代码：（递归→迭代）  
```cpp
class Solution {
private:
    vector<vector<int>> memo;
public:
    vector<int> helper(int numRows) {
        if (memo[numRows-1].size() > 0) return memo[numRows-1];
        if (numRows == 1) return {1};
        vector<int> prev = helper(numRows - 1);
        vector<int> ans (prev.size() + 1,1);
        for (int i = 1; i < ans.size() - 1; i++) {
            ans[i] = prev[i-1] + prev[i];
        }
        memo[numRows-1] = ans;
        return ans;
    }

    vector<vector<int>> generate(int numRows) {
        memo.resize(numRows);
        vector<vector<int>> ans;
        for (int i = 1; i <= numRows; i++) {
            ans.push_back(helper(i));
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ret(numRows);
        for (int i = 0; i < numRows; ++i) {
            ret[i].resize(i + 1);
            ret[i][0] = ret[i][i] = 1;
            for (int j = 1; j < i; ++j) {
                ret[i][j] = ret[i - 1][j] + ret[i - 1][j - 1];
            }
        }
        return ret;
    }
};
```