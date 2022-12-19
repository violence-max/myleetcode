题目描述：  
![image](/basical/gragh/image/image2.png)  
解题过程：  
忘记并查集是怎么写的了，并且忘记了并查集适用于什么场景了，强连通分量啊！！就是判断一个图的两个顶点是否相连接的，而且一定要注意，find函数是一个递归查询，通过当前节点所指向的父节点逐渐查找  
题解里面还有广度优先搜索和深度优先搜索两种方法，没太多时间写了，直接记下来就可以了，关键点是建立连接图并且设置一个访问数组 
代码：  
并查集： 
```cpp
class Solution {
private:
    vector<int> index;
public:
    int find(int x) {
        if (index[x] == x) {
            return x;
        }
        return index[x] = find(index[x]);
    }

    void merge(int x,int y) {
        x = find(x);
        y = find(y);
        index[y] = x;
    }

    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        index.resize(n);
        iota(index.begin(),index.end(),0);
        for (auto edge : edges) {
            merge(edge[0],edge[1]);
        }
        return find(index[source]) == find(index[destination]);
    }
};
```  
 广度优先搜索：  
```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<vector<int>> adj(n);
        for (auto &&edge : edges) {
            int x = edge[0], y = edge[1];
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }
        vector<bool> visited(n, false);
        queue<int> qu;
        qu.emplace(source);
        visited[source] = true;
        while (!qu.empty()) {
            int vertex = qu.front();
            qu.pop();
            if (vertex == destination) {
                break;
            }
            for (int next: adj[vertex]) {
                if (!visited[next]) {
                    qu.emplace(next);
                    visited[next] = true;
                }
            }
        }
        return visited[destination];
    }
};
```  
深度优先搜索：  
```cpp
class Solution {
public:
    bool dfs(int source, int destination, vector<vector<int>> &adj, vector<bool> &visited) {
        if (source == destination) {
            return true;
        }
        visited[source] = true;
        for (int next : adj[source]) {
            if (!visited[next] && dfs(next, destination, adj, visited)) {
                return true;
            }
        }
        return false;
    }

    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<vector<int>> adj(n);
        for (auto &edge : edges) {
            int x = edge[0], y = edge[1];
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }
        vector<bool> visited(n, false);
        return dfs(source, destination, adj, visited);
    }
};
```