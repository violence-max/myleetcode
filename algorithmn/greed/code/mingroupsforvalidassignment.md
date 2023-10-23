题目描述：

![image](/algorithmn/greed/image/image29.png)

解决过程：

第368场周赛t3，没做出来，数学问题没有想明白

代码：

```cpp
class Solution {
public:
    int minGroupsForValidAssignment(vector<int>& nums) {
        unordered_map<int,int> cnt;
        for (auto num : nums) {
            cnt[num]++;
        }
        int k = min_element(cnt.begin(), cnt.end(), [&](const auto& a, const auto& b){
            return a.second < b.second;
        })->second;
        for (; ; k--) {
            int ans = 0;
            for (auto [_, c] : cnt) {
                if (c / k < c % k) {
                    ans = 0;
                    break;
                }
                ans += (c + k) / (k + 1); // 最优的解法：按照更大的k+1分组
            }
            if (ans) {
                return ans;
            }
        }
    }
};
```