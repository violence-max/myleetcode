题目描述：

![image](/basicaldatastructure/trie/image/image4.png)

解题过程：

第106场双周赛t4，没做出来

这个题目需要分类讨论，具体讨论过程没有记录。我自己用字典树优化了一下时间复杂度

代码：

```cpp
class Node {
public:
    int val;
    int row; // 相当于end，用来标记是否到达尾部
    vector<Node*> v;
    Node(int _val) : val(_val) {
        row = -1;
        v.resize(2);
        for (int i = 0; i <= 1; ++i) {
            v[i] = nullptr;
        }
    }
};

class Trie {
public:
    Node* root;
    Trie() {
        root = new Node(-1);
    }

    int find(const vector<vector<int>>& grid, int row, int index, int n, Node* cur) {
        if (!cur) return -1;
        if (cur->row != -1) {
            return cur->row;
        }

        int x = grid[row][index];
        for (int j = 0; j <= 1; ++j) {
            // 当x==0时，两条路都需要尝试；当x==1时，只需要尝试0那条路
            
            if (x == 1 && j == 0) {
                continue;
            }
            int ans = find(grid, row, index + 1, n, cur->v[x ^ j]);
            if (ans != -1) {
                return ans;
            }
        }

        return -1;
    }

    int insert(const vector<vector<int>>& grid, int row, int n) {
        Node* cur = root;
        int x = find(grid, row, 0, n, cur);
        if (x != -1) {
            return x;
        }

        for (int i = 0; i < n; ++i) {
            int x = grid[row][i];
            if (!cur->v[x]) {
                cur->v[x] = new Node(x);
            }
            cur = cur->v[x];
        }
        cur->row = row;
        return -1;
    }
};

class Solution {
public:
    vector<int> goodSubsetofBinaryMatrix(vector<vector<int>>& grid) {
        // 本题需要对行数进行分类讨论，经过讨论后，可以得知如果答案存在，要么为1行要么为2行

        int m = grid.size(), n = grid[0].size();
        Trie t = Trie();
        for (int i = 0; i < m; ++i) {
            int cnt = 0;
            for (int j = n - 1; j >= 0; --j) {
                cnt += grid[i][j] == 0; 
            }
            if (cnt == n) {
                // 答案为1行时该行需要为全0

                return {i};
            }
            int row = t.insert(grid, i, n);
            if (row != -1) {
                // 答案为2行时，这两行按位与得到0
                // 使用字典树优化时间复杂度

                return {min(row,i), max(row,i)};
            }
        }

        return {};
    }
};
```