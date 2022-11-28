题目描述：  
![image](/algorithmn/tracebak/image/image10.png)    
解题过程：  
一开始以为就是把数组排好序然后去重复的子集选择，但是子序列长度要大于等于2，然而恍然大悟发现是子序列，也就是说不可以动数组。然后，我对子序列这个概念居然有非常大的误解，我以为必须连续，然后写了一个循环，对每个值进行处理，结果很丑陋。  
最终看了题解。  
题解里面有关去重复的部分一直不是很清楚，但是当我自己实际写了一遍整个搜寻子序列的过程之后我发现，居然真的像解答说的那样是不选择前者而选择后者。自己复现的时候又出糗了，在已经判断当前值大于上一个值得条件语句里执行完下一次递归之后弹出，然后—判断当前值是否和上一个值相等来执行下一次递归。这样就不是所有值都可以进行不选择当前值的下一次递归了。  
代码：  
```cpp
class Solution {
private:
    vector<vector<int>> ans;
    vector<int> temp;
public:
    void dfs(vector<int> nums,int index,int last) {
        if (index == nums.size()) {
            if (temp.size() >= 2) {
                ans.push_back(temp);
            }
            return;
        }
        if (nums[index] >= last) {
            temp.push_back(nums[index]);
            dfs(nums,index+1,nums[index]);
            temp.pop_back();
        }
        if (nums[index] != last) {
            dfs(nums,index+1,last);
        }
    }

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        dfs(nums,0,INT_MIN);
        return ans;
    }
};
```