题目描述：  
![image](/algorithmn/tracebak/image/image9.png)  
解题过程：  
以为这个题跟之前做过的一个去除重复元素是一致的，然后自信地写了一个循环加数组条件判断，结果出错。然后疯狂改，疯狂debug。在我细致地思考了算法过后，我发现只要在一进入回溯时添加判断是否重复的语句就可以很好地避免重复，但是我还没有写出终极版本。在测试阶段，我发现，直接越过重复返回会造成数据的损失，因此我添加了一行语句：**进入下一次回溯！！！**之后再返回。  
写的时候好像debug两次吧，都是很小的错误，一个判断数组越界问题写了大于0，应该大于等于0才对；一个应该判断上一个节点是否是false的判断条件写成判断这一个节点。离谱。  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> ans;
    vector<bool> used;
    vector<int> temp;
public:
    void dfs(vector<int> nums,int index) {
        if (index == nums.size()) {
            ans.push_back(temp);
            return;
        } else if (index-1 >= 0 && nums[index] == nums[index-1] && !used[index-1]) {
            dfs(nums,index+1);
            return; 
        }
        temp.push_back(nums[index]); used[index] = true;
        dfs(nums,index+1);
        temp.pop_back(); used[index] = false;
        dfs(nums,index+1);
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        used = vector<bool> (nums.size(),false);
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return ans;
    }
};
```