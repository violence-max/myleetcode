题目描述：  
![image](/basical/array/image/image70.png)  
解决过程：  
没做出来，想得比较复杂，以为是回溯  
- 暴力
1. 构建邻接表
2. 遍历每个花园，对于每个花园，标记出其邻接花园已经使用过的颜色，再剩余的可以使用的颜色里面挑取一个颜色出来使用即可
3. 由于每个花园最多有3条路可以进入或者出去，所以一定存在一个符合条件的答案  
代码：  
```cpp
class Solution {
public:
    vector<int> gardenNoAdj(int n, vector<vector<int>>& paths) {
        vector<vector<int>> adj (n);
        vector<int> ans (n);
        for (const auto &path : paths) {
            int gardern1 = path[0] - 1, gardern2 = path[1] - 1;
            adj[gardern1].push_back(gardern2);
            adj[gardern2].push_back(gardern1);
        }
        for (int i = 0; i < n; i++) {
            vector<bool> colored (5, false);
            for (auto &vertext : adj[i]) {
                colored[ans[vertext]] = true;
            }
            for (int j = 1; j <= 4; j++) {
                if (!colored[j]) {
                    ans[i] = j;
                    break;
                }
            }
        }
        return ans;
    }
};
```