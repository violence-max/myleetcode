题目描述：  
![image](/basical/gragh/image/image10.png)  
解决过程：  
没看懂题目，看了题解才发现最大网络秩是指与两个城市的直接连接的道路的数目，即图中绿色的道路  
- 枚举：计算度数+邻接矩阵+O(n^2)for循环
- 贪心：求出度数最大的firstarr和次大的secondarr再分类讨论  
代码：（枚举→贪心）  
```cpp
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        vector<vector<bool>> connect (n, vector<bool> (n,false));
        vector<int> degree (n);
        for (auto& road : roads) {
            connect[road[0]][road[1]] = true;
            connect[road[1]][road[0]] = true;
            degree[road[0]]++;
            degree[road[1]]++;
        }

        int maxRank = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int rank = degree[i] + degree[j] - ((connect[i][j]) ? 1 : 0);
                // cout << i << ' ' << j << ' ' << rank << endl;
                maxRank = max(maxRank, rank);
            }
        }
        return maxRank;
    }
};
```  
```cpp
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        vector<vector<bool>> connect(n, vector<bool>(n, false));
        vector<int> degree(n);
        for (auto road : roads) {
            int x = road[0], y = road[1];
            connect[x][y] = true;
            connect[y][x] = true;
            degree[x]++;
            degree[y]++;
        }

        int first = -1, second = -2;
        vector<int> firstArr, secondArr;
        for (int i = 0; i < n; ++i) {
            if (degree[i] > first) {
                second = first;
                secondArr = firstArr;
                first = degree[i];
                firstArr.clear();
                firstArr.emplace_back(i);
            } else if (degree[i] == first) {
                firstArr.emplace_back(i);
            } else if (degree[i] > second){
                secondArr.clear();
                second = degree[i];
                secondArr.emplace_back(i);
            } else if (degree[i] == second) {
                secondArr.emplace_back(i);
            }
        }
        if (firstArr.size() == 1) {
            int u = firstArr[0];
            for (int v : secondArr) {
                if (!connect[u][v]) {
                    return first + second;
                }
            }
            return first + second - 1;
        } else {
            int m = roads.size();
            if (firstArr.size() * (firstArr.size() - 1) / 2 > m) {
                return first * 2;
            }
            for (int u: firstArr) {
                for (int v: firstArr) {
                    if (u != v && !connect[u][v]) {
                        return first * 2;
                    }
                }
            }
            return first * 2 - 1;
        }
    }
};
```