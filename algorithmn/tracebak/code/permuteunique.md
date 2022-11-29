题目描述：  
![image](/algorithmn/tracebak/image/image12.png)  
解题过程：  
这道题目没有做出来哈哈，不知道是什么原因，我一开始去重复的想法是不能和前一个值相等，后面是当前要交换的值不能和索引值相等，但是最后一个测试用例没有通过，很奇怪。  
其实一开始甚至把下一次回溯写错了，好久才发现，就是要把数组分成左右排序好的两个部分，但是我在下一次递归的时候直接进入了i+1层，而不是index+1层，index左侧才是排好的数据。  
打算把这个题的代码部分留着，等着以后debug。  
代码：  
自己：（待debug） 
```cpp
class Solution {
private:
    vector<vector<int>> ans;
    bool flag;
public:
    void dfs(vector<int> &nums,int index) {
        if (index == nums.size()) {
            ans.push_back(nums);
            return;
        }
        for (int i = index; i < nums.size(); i++) {
            if (!flag && ((nums[i] == nums[index] && i != index) || (i-1 >= 0 && nums[i] == nums[i-1]))) continue;
            flag = true;
            swap(nums[i],nums[index]);
            dfs(nums,index+1);
            flag = false;
            swap(nums[i],nums[index]);
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return ans;
    }
};
```  
题解：  
```cpp
class Solution {
private:
    vector<vector<int>> ans;
    vector<int> temp;
    vector<bool> used;
public:
    void dfs(vector<int> &nums,int index) {
        if (index == nums.size()) {
            ans.push_back(temp);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (used[i] || (i-1 >= 0 && nums[i] == nums[i-1] && !used[i-1])) continue;
            used[i] = true;
            temp.push_back(nums[i]);
            dfs(nums,index+1);
            used[i] = false;
            temp.pop_back();
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        used = vector<bool> (nums.size(),false);
        dfs(nums,0);
        return ans;
    }
};
```