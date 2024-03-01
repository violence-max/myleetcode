题目描述：

![image](/basicaldatastructure/setandmap/image/image15.png)

解决过程：

没做出来

哈希表法求数组中两个数最大异或值板子题

代码：

```cpp
class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        int mxx = *max_element(nums.begin(), nums.end());
        int hightbit = mxx ? 31 - __builtin_clz(mxx) : -1;
        int mask = 0, ans = 0;
        unordered_set<int> seen;
        for (int i = hightbit; i >= 0; --i) {
            mask = mask | (1 << i);
            int new_ans = ans | (1 << i);
            seen.clear();
            for (int x : nums) {
                x = x & mask;
                if (seen.count(new_ans ^ x)) {
                    ans = new_ans;
                    break;
                }
                seen.insert(x);
            }
        }
        return ans;
    }
};
```