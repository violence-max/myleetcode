题目描述：  
![image](/algorithmn/tracebak/image/image5.png)  
解题过程：  
烦，以为自己可以做出来的，结果没有。最恶心的是，我刚刚看完代码随想录对于组合总和I的剪纸的一个超级棒的思想，就是横向循环遍历，纵向递归遍历，然而我在书写这个思想的时候居然把循环里的变量i写成了index，我直接无语，甚至包括判断条件我都写的index，我真的无敌了。  
这个题目正常的情况下我应该花一分钟的时间把含有重复解的部分答案给拿到，然后思考去除重复解。  
看了代码随想录，无敌，用一个额外的数组来递归保存数组中每个值的使用情况。  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> res;
    vector<int> temp;
public:
    void dfs(vector<int> candidates,int sum,int index,int target,vector<bool> used) {
        if (target == sum) {
            res.push_back(temp);
            return;
        }
        for (int i = index; i < candidates.size() && sum + candidates[i] <= target; i++) {
            if (i > 0 && candidates[i] == candidates[i-1] && !used[i-1]) continue;
            sum += candidates[i];  temp.push_back(candidates[i]);
            used[i] = true;
            dfs(candidates,sum,i+1,target,used);
            sum -= candidates[i]; temp.pop_back();
            used[i] = false;
        }
    } 
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used (candidates.size(),false);
        sort(candidates.begin(),candidates.end());
        dfs(candidates,0,0,target,used);
        return res;
    }
};
```