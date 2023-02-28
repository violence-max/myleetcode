题目描述：  
![image](/basical/gragh/image/image7.png)  
解决过程：  
5分钟ac，题解给了深度优先搜索的方法，记录一下  
代码：（广度优先→深度优先）  
```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        unordered_map<int,vector<int>> pre;
        vector<int> degree (numCourses);
        for (auto& prerequisite : prerequisites) {
            pre[prerequisite[0]].push_back(prerequisite[1]);
            ++degree[prerequisite[1]];
        }
        int validcourses = 0;
        queue<int> q;
        for (int i = 0; i < degree.size(); i++) {
            if (degree[i] == 0) q.push(i);
        }
        while (!q.empty()) {
            int node = q.front(); q.pop();
            for (const auto& p : pre[node]) {
                --degree[p];
                if (degree[p] == 0) q.push(p);
            }
        }
        return !any_of(degree.begin(),degree.end(),[](int i) {return i > 0;});
    }
};
```  
```cpp
class Solution {
private:
    vector<vector<int>> edges;
    vector<int> visited;
    bool valid = true;

public:
    void dfs(int u) {
        visited[u] = 1;
        for (int v: edges[u]) {
            if (visited[v] == 0) {
                dfs(v);
                if (!valid) {
                    return;
                }
            }
            else if (visited[v] == 1) {
                valid = false;
                return;
            }
        }
        visited[u] = 2;
    }

    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        edges.resize(numCourses);
        visited.resize(numCourses);
        for (const auto& info: prerequisites) {
            edges[info[1]].push_back(info[0]);
        }
        for (int i = 0; i < numCourses && valid; ++i) {
            if (!visited[i]) {
                dfs(i);
            }
        }
        return valid;
    }
};
```