题目描述：  
![image](/algorithmn/dynamic_programming/image/image55.png)  
解题过程：  
没做出来  
- 数位dp  
代码：  
```cpp
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        int n = req_skills.size(), m = people.size();
        unordered_map<string, int> skill_index;
        for (int i = 0; i < n; ++i) {
            // 建立需求技能到索引的映射，方便掩码的使用
            skill_index[req_skills[i]] = i;
        }
        vector<vector<int>> dp(1 << n); // dp[i]使包含技能i的最小团队
        for (int i = 0; i < m; ++i) {
            int cur_skill = 0;
            for (string& s : people[i]) {
                // 计算技能掩码
                cur_skill |= 1 << skill_index[s];
            }
            for (int prev = 0; prev < dp.size(); ++prev) {
                // 遍历寻找之前的技能，如果之前的技能为空实际上是没有意义的，而且必须要已经找到人了
                if (prev > 0 && dp[prev].empty()) {
                    continue;
                }
                int comb = prev | cur_skill;
                if (comb == prev) {
                    continue;
                }
                if (dp[comb].empty() || dp[prev].size() + 1 < dp[comb].size()) {
                    // 1. comb技能还没找到人；2.comb的技能已经有人了，但是相比之前的技能再加1实在是比较多，不符合最小团队要求
                    dp[comb] = dp[prev];
                    dp[comb].push_back(i);
                }
            }
        }
        return dp[(1 << n) - 1];
    }
};
```