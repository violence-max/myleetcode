题目描述：

![image](/basicaldatastructure/binary_tree/image/image59.png)

解决过程：

没做出来

- 树上倍增算法

代码：

```cpp
class TreeAncestor {
public:
    const int Log = 16;
    vector<vector<int>> node2p;
    TreeAncestor(int n, vector<int>& parent) {
        node2p = vector<vector<int>> (n, vector<int> (Log, -1));
        for (int i = 0; i < n; ++i) {
            node2p[i][0] = parent[i];
        }
        for (int j = 1; j < Log; ++j) {
            // 倍增算法，node2p[i][j]表示节点i的第2^j个祖先节点

            for (int i = 0; i < n; ++i) {
                if (node2p[i][j - 1] != -1) {
                    node2p[i][j] = node2p[node2p[i][j-1]][j-1]; // 倍增构造
                }
            }
        }
    }

    int getKthAncestor(int node, int k) {
        for (int j = 0; j < Log; ++j) {
            // 由于k最大值为50000，因此k的二进制至多到第15位（从0开始计数）
            // 枚举二进制位数，如果k对应的位数为1，说明在求node的第2^j个祖先节点

            if ((k >> j) & 1) {
                node = node2p[node][j];
                if (node == -1) {
                    return -1;
                }
            }
        }
        return node;
    }
};
```