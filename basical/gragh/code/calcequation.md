题目描述：  
![image](/basical/gragh/image/image8.png)  
解决过程：  
知道是用并查集，但是没有掌握到并查集的精髓以及没体会到题目的精髓  
算法要点：  
1. 使用并查集，用数字代表每个string
2. 使用并查集中的路径压缩策略，每个并查集中只有叶节点到根节点的一条路径，每次往并查集中添加元素的时候，要么生成另外一个并查集要么在原来的并查集中更新根节点
3. 每个节点的权值代表节点与根节点的比值
4. 在递归查询find的时候需要将节点到父节点的权值更新为到根节点的权值，并且将节点的父节点更改为根节点
5. 一边查询一边修改节点是并查集的特色  
代码:  
```cpp
class UnionFind {
private:
    vector<int> parent;
    vector<double> weight;
public:
    UnionFind(int n) {
        parent.resize(n); weight.resize(n);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            weight[i] = 1.0;
        }
    } 

    int find(int x) {
        if (x != parent[x]) {
            int origin = parent[x];
            parent[x] = find(parent[x]);
            weight[x] *= weight[origin];
        }
        return parent[x];
    }

    double isConnected(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) {
            return weight[x] / weight[y];
        } else return -1.0;
    }

    void merge(int x, int y, double value) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return;
        parent[rootX] = rootY;
        weight[rootX] = weight[y] * value / weight[x];
    }
};

class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        int equationsSize = equations.size();
        unordered_map<string,int> map;
        UnionFind uf = UnionFind(2 * equationsSize);
        int id = 0, index = 0;
        for (auto& equation : equations) {
            string val1 = equation[0], val2 = equation[1];
            if (!map.count(val1)) {
                map[val1] = id++;
            }
            if (!map.count(val2)) {
                map[val2] = id++;
            }
            uf.merge(map[val1],map[val2],values[index++]);
        }
        vector<double> res (queries.size(), -1.0);
        int _index = 0;
        for (auto& querie : queries) {
            string _val1 = querie[0], _val2 = querie[1];
            res[_index++] = (map.count(_val1) && map.count(_val2)) ? uf.isConnected(map[_val1], map[_val2]) : -1.0;
        }
        return res;
    }
};
```