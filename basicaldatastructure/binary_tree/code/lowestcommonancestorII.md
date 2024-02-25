题目描述：

![image](/basicaldatastructure/binary_tree/image/image69.png)

解决过程：

没做出来，主要是想树上倍增算法和lca解法去了，没思考二叉搜索树的性质，一次遍历分叉点这个思路真是让人眼前一亮

代码：

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* ancestor = root;
        while (true) {
            if (p->val < ancestor->val && q->val < ancestor->val) {
                ancestor = ancestor->left;
            } else if (p->val > ancestor->val && q->val > ancestor->val) {
                ancestor = ancestor->right;
            } else {
                break;
            }
        }
        return ancestor;
    }
};
```