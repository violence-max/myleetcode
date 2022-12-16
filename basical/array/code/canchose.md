题目描述：  
![image](/basical/array/image/image23.png)  
解题过程：  
简单一批，逐渐循环group数组里面的容器然后一个一个比较就可以了，类似于双指针的思路  
代码：  
```cpp
class Solution {
public:
    bool matched(vector<int> m,vector<int> nums,int start) {
        int index = start;
        for (int n : m) {
            if (index >= (int)nums.size() || nums[index] != n) {
                return false;
            } else {
                index++;
            }
        }
        return true;
    }
    bool canChoose(vector<vector<int>>& groups, vector<int>& nums) {
        int i = 0;
        int n = (int)groups.size();
        int count = 0;
        for (auto v : groups) {
            while (i < (int)nums.size()) {
                if (!matched(v,nums,i)) {
                    i++;
                } else {
                    i += (int)v.size();
                    count++;
                    break; 
                }
            }
        }
        return (count == n) ? true : false;
    }
};
```