题目描述：  
![image](/basical/array/image/image23.png)  
解题过程：  
* 模拟比较:
1. 对group进行循环，逐一在nums数组中进行比较，比较不成功则比较的起始位置加1，否则加上正在比较的group的元素的子数组的大小
2. 统计成功比较的元素个数count，若最终count == group.size()，则可以返回true
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