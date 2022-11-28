题目描述：  
![image](/algorithmn/tracebak/image/image11.png)  
解题过程：  
笑死又是根本不会。  
题解是真的清晰，非常清晰的一个枚举。假设当前位置之前的部分已经排好了，然后排剩余的部分，剩余的部分里面不能有之前用过的数字。这个思路就很牛逼。实现起来是真的太骚了，维护了左右两个子数组，居然是用交换实现的？？？离谱  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> ans;
public:
    void dfs(vector<int> output,int index) {
        if (index == output.size()) {
            ans.push_back(output);
        }
        for (int i = index; i < output.size(); i++) {
            swap(output[i],output[index]);
            dfs(output,index+1);
            swap(output[i],output[index]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        dfs(nums,0);
        return ans;
    }
};
```