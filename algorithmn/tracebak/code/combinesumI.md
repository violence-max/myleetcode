题目描述：  
![image](/algorithmn/tracebak/image/image4.png)  
解题过程：  
是真的不会，因为一个数字可以重复利用，一开始还想着能不能用数学的方法列举出来，我竟然发现根本不可以。然后看了题解，看了图之后以为恍然大悟了，当然也看了代码哈哈哈，就来写，写了好久没写出来我靠，又看一次题解，这次没看代码，直接看图，发现自己犯下了一个很严重的错误—根本没有理解题解。我的解法是以数组的第一个值为根节点，而题解是以一个空节点为跟节点，左边是不选择当前节点直接来到下一个节点，右边是选择当前节点再继续来到当前节点。真的赖皮蛇。
自己还添加了一个亮点吧算是，就是先把数组升序排序然后再寻找，我觉得这样子可以减去很多不必要的查找。  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> res;
    vector<int> temp;
public:
    void dfs(vector<int> candidates,int target,int index) {
        if (target == 0) {
            res.push_back(temp); return;
        } else if ((index < candidates.size() && candidates[index] > target) || target < 0 || index >= candidates.size()) return;
        dfs(candidates,target,index+1);
        if (target - candidates[index] >= 0) {
            temp.push_back(candidates[index]);
            dfs(candidates,target-candidates[index],index);
        }
        temp.pop_back();
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        dfs(candidates,target,0);
        return res;
    }
};
```