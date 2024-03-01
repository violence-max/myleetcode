题目描述：

![image](/basicaldatastructure/stackandquere/code/mostcompetitive.md)

解题过程：

没做出来

- 单调栈

代码：

```cpp
int n, d;
vector<int> stk;
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        n = nums.size();
        stk.clear();
        d = n - k;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && stk.back() > nums[i] && d > 0) {
                // 转化为删除n-k个优先级较低的数字
                // 升序单调栈保证了子数组的数据的连续性
                stk.pop_back();
                d--;
            }
            stk.push_back(nums[i]);
        }

        if (d > 0) {
            while (d--) {
                stk.pop_back();
            }
        }

        vector<int> res;
        while (!stk.empty()) {
            res.push_back(stk.back());
            stk.pop_back();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```