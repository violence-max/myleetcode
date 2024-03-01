题目描述：

![image](/basicaldatastructure/binary_tree/image/image67.png)

解决过程：

没做出来

- 递归

代码：

```cpp
class Solution {
public:
    pair<TreeNode*, int> f(TreeNode* root) {
        if (!root) {
            return {root, 0};
        }

        auto left = f(root->left);
        auto right = f(root->right);

        if (left.second > right.second) {
            return {left.first, left.second + 1};
        } else if (left.second < right.second) {
            return {right.first, right.second + 1};
        } else {
            return {root, left.second + 1};
        }
    }
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        return f(root).first;
    }
};
```