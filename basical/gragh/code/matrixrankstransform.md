题目描述：  
![image](/basical/gragh/image/image5.png)  
遇到困难我直接两手一摊  
并查集+拓扑排序：  
1. 并查集的构建：typedef一开始还写错了，不过无所谓。主要是find和merge。因为这里构建并查集的背景是下标，因此并查集里的元素是pair<int,int>，find的书写非常简单但是merge的书写需要一个大小与矩阵一致的size来辅助。在初始化并查集的时候就需要把size构建好—每个位置的初值为1，表示这个位置只有一个元素。而当遇到两个相同元素进行合并时，就直接把size小的元素改成size大的就好，同时修改size大的元素的size，具体见代码
2. 拓扑排序的准备—合并并查集：这道题目里面同一行和同一列会出现很多重复元素，然而这些重复元素具有相同的秩，因此需要把这些重复元素的位置给合并起来，有一个共同的father，然后计算这个father的秩，最终其它的元素共用这个秩就可以了
3. 拓扑排序的关键点—图的构建：对于每一行和每一列，因为在第2点中我们已经对重复元素进行合并了，因此我们只需要将去重元素进行排序就可以得到该行或者该列的元素的大小关系了。根据该大小关系，我们可以构建入度图和单向连接图，对于排序后序号在1以后的元素，其入度加1，并且相应地构建单向连接图
4. 拓扑排序的重投戏—广度优先搜索：我们使用一个队列，将入度为0的元素的位置入队，每次出队时，遍历这个位置的单向连接图，得到该元素指向的位置，然后使被指向的位置的入度减1，如果入度被减到0，则说明被指向的位置的秩已经算好，就是指向其的位置的秩加1
5. 最终我们对于矩阵的每个位置，找到father，然后把秩给复制过来就可以了
题解的代码与我的差别在于：  
1. 题解的单向连接表的数据结构选用的是哈希表，我是vector
2. 题解对于那些无效节点（不是father的重复节点）进行了处理，没让他们入队
3. 题解在遍历单向连接表时每次都对被遍历的节点的秩进行加1操作，而我是只有当被遍历到的节点的入度为0时才进行赋值操作
亮点：  
1. 二维矩阵的一维化下标写法
2. 哈希表和队列的emplace方法
3. 二维矩阵的横向遍历和纵向遍历使用一个bool变量来统一化  
代码：（我的→题解）  
```cpp
typedef pair<int,int> p;

class UnionFind {
private:
    int m;
    int n;
    vector<vector<p>> root;
    vector<vector<int>> size;
public:
    UnionFind(int m, int n) {
        this->m = m;
        this->n = n;
        root.resize(m,vector<p> (n));
        size.resize(m,vector<int> (n));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                root[i][j] = make_pair(i,j);
                size[i][j] = 1;                
            }
        }
    }

    p find(int i, int j) {
        auto [ri,rj] = root[i][j];
        if (ri == i && rj == j) {
            return make_pair(ri,rj);
        }
        return find(ri,rj);
    }

    void merge(int i1, int j1, int i2, int j2) {
        auto [ri1,rj1] = find(i1,j1);
        auto [ri2,rj2] = find(i2,j2);
        if (ri1 != ri2 || rj1 != rj2) {
            if (size[ri1][rj1] >= size[ri2][rj2]) {
                root[ri2][rj2] = make_pair(ri1,rj1);
                size[ri1][rj1] += size[ri2][rj2];
            } else {
                root[ri1][rj1] = make_pair(ri2,rj2);
                size[ri2][rj2] += size[ri1][rj1];
            }
        }
    }
};

class Solution {
private:
    vector<vector<int>> matrix;
public:
    void merge(int m, int n, UnionFind& uf,bool heng) {
        for (int i = 0; i < m; i++) {
            unordered_map<int,vector<p>> map;
            for (int j = 0; j < n; j++) {
                int row = (heng) ? i : j;
                int col = (heng) ? j : i;
                map[matrix[row][col]].emplace_back(make_pair(row,col));
            }
            for (auto& [_ , plist] : map) {
                auto [i1,j1] = plist[0];
                for (int k = 1; k < plist.size(); k++) {
                    auto [i2,j2] = plist[k];
                    uf.merge(i1,j1,i2,j2);
                }
            }
        }
    }

    void constrcutgragh(vector<vector<int>>& degree, vector<vector<p>>& adj, int m, int n, int col, UnionFind& uf, bool heng) {
        for (int i = 0; i < m; i++) {
            unordered_map<int,p> map;
            set<int> s;
            for (int j = 0; j < n; j++) {
                int row = (heng) ? i : j;
                int c = (heng) ? j : i;
                map[matrix[row][c]] = make_pair(row,c);
                s.insert(matrix[row][c]);
            }
            vector<int> sortedarray (s.begin(),s.end());
            for (int k = 1; k < sortedarray.size(); k++) {
                auto [ri1,rj1] = map[sortedarray[k-1]];
                auto [ri2,rj2] = map[sortedarray[k]];
                auto [i1,j1] = uf.find(ri1,rj1);
                auto [i2,j2] = uf.find(ri2,rj2);
                degree[i2][j2]++;
                adj[i1*col+j1].emplace_back(make_pair(i2,j2));
            }
        }
    }

    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        UnionFind uf = UnionFind(m,n);
        this->matrix = matrix;
        merge(m,n,uf,true); merge(n,m,uf,false);
        vector<vector<int>> degree (m,vector<int> (n,0));
        vector<vector<p>> adj (m*n,vector<p> {});
        constrcutgragh(degree,adj,m,n,n,uf,true); constrcutgragh(degree,adj,n,m,n,uf,false);
        queue<p> q;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (degree[i][j] == 0) {
                    q.push(make_pair(i,j));
                }
            }
        }
        vector<vector<int>> ranks (m, vector<int> (n,1));
        while (!q.empty()) {
            auto [i1,j1] = q.front();
            q.pop();
            for (auto [i2,j2] : adj[i1*n+j1]) {
                degree[i2][j2]--;
                if (degree[i2][j2] == 0) {
                    q.push(make_pair(i2,j2));
                    ranks[i2][j2] = ranks[i1][j1]+1;
                }
            }
        }
        vector<vector<int>> ans (m, vector<int> (n));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                auto [row,col] = uf.find(i,j);
                ans[i][j] = ranks[row][col];
            }
        }
        return ans;
    }
};
```
```cpp
typedef pair<int, int> pii;

class UnionFind {
    int m, n;
    vector<vector<pii>> root;
    vector<vector<int>> size;

public:
    UnionFind(int m, int n) {
        this->m = m;
        this->n = n;
        this->root = vector<vector<pii>>(m, vector<pii>(n));
        this->size = vector<vector<int>>(m, vector<int>(n));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                root[i][j] = make_pair(i, j);
                size[i][j] = 1;
            }
        }
    }

    pii find(int i, int j) {
        pii rootArr = root[i][j];
        int ri = rootArr.first, rj = rootArr.second;
        if (ri == i && rj == j) {
            return rootArr;
        }
        return find(ri, rj);
    }

    void Uni(int i1, int j1, int i2, int j2) {
        auto [ri1, rj1] = find(i1, j1);
        auto [ri2, rj2] = find(i2, j2);
        if (ri1 != ri2 || rj1 != rj2) {
            if (size[ri1][rj1] >= size[ri2][rj2]) {
                root[ri2][rj2] = make_pair(ri1, rj1);
                size[ri1][rj1] += size[ri2][rj2];
            } else {
                root[ri1][rj1] = make_pair(ri2, rj2);
                size[ri2][rj2] += size[ri1][rj1];
            }
        }
    }
};

class Solution {
public:
    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        UnionFind uf(m, n);
        for (int i = 0; i < m; i++) {
            unordered_map<int, vector<pii>> num2indexList;
            for (int j = 0; j < n; j++) {
                num2indexList[matrix[i][j]].emplace_back(i, j);
            }
            for (auto [_, indexList] : num2indexList) {
                auto [i1, j1] = indexList[0];
                for (int k = 1; k < indexList.size(); k++) {
                    auto [i2, j2] = indexList[k];
                    uf.Uni(i1, j1, i2, j2);
                }
            }
        }
        for (int j = 0; j < n; j++) {
            unordered_map<int, vector<pii>> num2indexList;
            for (int i = 0; i < m; i++) {
                num2indexList[matrix[i][j]].emplace_back(i, j);
            }
            for (auto [_, indexList] : num2indexList) {
                auto [i1, j1] = indexList[0];
                for (int k = 1; k < indexList.size(); k++) {
                    auto [i2, j2] = indexList[k];
                    uf.Uni(i1, j1, i2, j2);
                }
            }
        }

        vector<vector<int>> degree(m, vector<int>(n));
        unordered_map<int, vector<pii>> adj;
        for (int i = 0; i < m; i++) {
            unordered_map<int, pii> num2index;
            for (int j = 0; j < n; j++) {
                num2index[matrix[i][j]] = make_pair(i, j);
            }
            vector<int> sortedArray;
            for (auto [key, _] : num2index) {
                sortedArray.emplace_back(key);
            }
            sort(sortedArray.begin(), sortedArray.end());
            for (int k = 1; k < sortedArray.size(); k++) {
                auto [i1, j1] = num2index[sortedArray[k - 1]];
                auto [i2, j2] = num2index[sortedArray[k]];
                auto [ri1, rj1] = uf.find(i1, j1);
                auto [ri2, rj2] = uf.find(i2, j2);
                degree[ri2][rj2]++;
                adj[ri1 * n + rj1].emplace_back(ri2, rj2);
            }
        }
        for (int j = 0; j < n; j++) {
            unordered_map<int, pii> num2index;
            for (int i = 0; i < m; i++) {
                num2index[matrix[i][j]] = make_pair(i, j);
            }
            vector<int> sortedArray;
            for (auto [key, _] : num2index) {
                sortedArray.emplace_back(key);
            }
            sort(sortedArray.begin(), sortedArray.end());
            for (int k = 1; k < sortedArray.size(); k++) {
                auto [i1, j1] = num2index[sortedArray[k - 1]];
                auto [i2, j2] = num2index[sortedArray[k]];
                auto [ri1, rj1] = uf.find(i1, j1);
                auto [ri2, rj2] = uf.find(i2, j2);
                degree[ri2][rj2]++;
                adj[ri1 * n + rj1].emplace_back(ri2, rj2);
            }
        }

        unordered_set<int> rootSet;
        vector<vector<int>> ranks(m, vector<int>(n));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                auto [ri, rj] = uf.find(i, j);
                rootSet.emplace(ri * n + rj);
                ranks[ri][rj] = 1;
            }
        }
        queue<pii> q;
        for (int val : rootSet) {
            if (degree[val / n][val % n] == 0) {
                q.emplace(val / n, val % n);
            }
        }
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();
            for (auto [ui, uj] : adj[i * n + j]) {
                degree[ui][uj]--;
                if (degree[ui][uj] == 0) {
                    q.emplace(ui, uj);
                }
                ranks[ui][uj] = max(ranks[ui][uj], ranks[i][j] + 1);
            }
        }
        vector<vector<int>> res(m, vector<int>(n));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                auto [ri, rj] = uf.find(i, j);
                res[i][j] = ranks[ri][rj];
            }
        }
        return res;
    }
};
```