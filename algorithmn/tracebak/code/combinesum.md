题目描述：  
![image](/algorithmn/tracebak/image/image2.png)    
解题过程：  
写了挺久的，强行用回溯给撸出来了  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> res;
    vector<int> temp;
    int sum = 0;
public:
    void dfs(int cur,int n,int k) {
        if (sum == n && temp.size() == k) {
            res.push_back(temp);
            return;
        } else if (sum > n || (temp.size() == k && sum < n) || cur > 9) return;
        sum += cur; temp.push_back(cur);
        dfs(cur+1,n,k);
        sum -= cur; temp.pop_back();
        dfs(cur+1,n,k);
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(1,n,k);
        return res;
    }
};
```