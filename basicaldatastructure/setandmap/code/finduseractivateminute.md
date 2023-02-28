题目描述：  
![image](/basicaldatastructure/setandmap/image/image2.png)  
解题过程：  
so easy  
代码：  
```cpp
class Solution {
public:
    vector<int> findingUsersActiveMinutes(vector<vector<int>>& logs, int k) {
        unordered_map<int,unordered_set<int>> map;
        vector<int> ans(k,0);
        for (auto &v : logs) {
            if (map[v[0]].find(v[1]) == map[v[0]].end()) {
                map[v[0]].insert(v[1]);
            }
        }
        for (auto& m : map) {
            int miniute = m.second.size();
            ans[miniute-1] += 1;
        }
        return ans;
    }
};
```