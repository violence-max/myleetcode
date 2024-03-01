题目描述：

![image](/basicaldatastructure/binary_tree/image/image57.png)

解决过程：

写了15分钟左右

- 深度优先搜索

代码：

```cpp
class Solution {
public:
    TreeNode* dfs(TreeNode* root, int limit, int sum) {
        if (!root) return nullptr;
        sum += root->val;
        if (!root->left && !root->right) {
            return sum >= limit ? root : nullptr;
        }

        bool left_flag = false, right_flag = false;
        if (root->left) {
            root->left = dfs(root->left, limit, sum);
            if (root->left) {
                left_flag = true;
            }
        }
        if (root->right) {
            root->right = dfs(root->right, limit, sum);
            if (root->right) {
                right_flag = true;
            }
        }
        return (!left_flag && !right_flag) ? nullptr : root;
    }

    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        return dfs(root, limit, 0);
    }
};
```