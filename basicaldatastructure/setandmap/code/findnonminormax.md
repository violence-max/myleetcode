题目描述：

![image](/basicaldatastructure/setandmap/image/image14.png)

解题过程：

第349场周赛t1，2分钟ac

代码：

```cpp
class Solution {
public:
    int findNonMinOrMax(vector<int>& nums) {
        int n = nums.size();
        if (n == 2) {
            return -1;
        }
        
        unordered_set<int> s;
        int mn = INT_MAX, mx = INT_MIN;
        for (auto i : nums) {
            if (i < mn) {
                mn = i;
            }
            if (i > mx) {
                mx = i;
            }
            s.insert(i);
        }
        for (auto v : s) {
            if (v != mn && v != mx) {
                return v;
            }
        }
        return -1;
    }
};
```