题目描述：  
![image](/algorithmn/tracebak/image/image8.png)  
解题过程：  
可能状态不太好，明明知道只是选或者不选那么简单而已，居然没有做出来，分割子字符串这种问题都做出来了，这个做不出来，看来对回溯的理解还是太少了。甚至对位运算的理解还没透彻，脑子里不能思考出一个完整的代码，唉。  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> ans;
    vector<int> temp;
public:
    void dfs(vector<int> nums,int index) {
        if (index == nums.size()) {
            ans.push_back(temp); return;
        }
        temp.push_back(nums[index]);
        dfs(nums,index+1);
        temp.pop_back();
        dfs(nums,index+1);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        dfs(nums,0);
        return ans;
    }
};
```