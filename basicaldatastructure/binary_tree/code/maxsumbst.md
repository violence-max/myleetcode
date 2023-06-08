题目描述：

![image](/basicaldatastructure/binary_tree/image/image58.png)

解决过程：

没做出来

- 深度优先搜索

代码：

```cpp
class Solution {
public:
    struct Entry {
        bool isBST; // 是否是二叉搜索树
        int min_val;
        int max_val;
        int sum; // 键值总和
        Entry(bool _isBST, int _min_val, int _max_val, int _sum) : isBST(_isBST), min_val(_min_val), max_val(_max_val), sum(_sum) {}
    };

    int res;

    int maxSumBST(TreeNode* root) {
        res = 0;
        dfs(root);
        return res;
    }

    Entry dfs(TreeNode* node) {
        if (!node) {
            return Entry(true, INT_MAX, INT_MIN, 0); // 空节点默认为二叉搜索树
        }

        // 深度优先搜索，每个节点都遍历一次
        auto left = dfs(node->left);
        auto right = dfs(node->right);

        if (left.isBST && right.isBST && node->val > left.max_val && node->val < right.min_val) {
            // 以节点node为根节点找到了一颗二叉搜索树

            int sum = left.sum + right.sum + node->val;
            res = max(res, sum);
            return Entry(true, min(left.min_val, node->val), max(right.max_val, node->val), sum);
        } else {
            return Entry(false, 0, 0, 0);
        }
    }
};
```