题目描述：

![image](/basical/simulation/image/image7.png)

解决过程：

第108场双周赛t2，32分钟

代码：

```cpp
class Solution {
public:
    vector<int> relocateMarbles(vector<int>& nums, vector<int>& moveFrom, vector<int>& moveTo) {
        int n = nums.size();
        vector<int> ans;
        unordered_map<int, int> cnt;
        for (int i = 0; i < n; ++i) {
            cnt[nums[i]]++;
        }
        int l = moveFrom.size();
        for (int i = 0; i < l; ++i) {
            cnt.erase(moveFrom[i]);
            cnt[moveTo[i]]++;
            // for (const auto& p : cnt) {
                // cout << p.first << ' ' << p.second << endl;
            // }
        }
        for (const auto& p : cnt) {
            int x = p.first;
            // cout << x << endl;
            ans.push_back(x);
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```