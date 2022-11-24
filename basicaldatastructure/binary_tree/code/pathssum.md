题目描述：  
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a88bfb58-5c42-471b-986d-dafb76c9742b/Untitled.png)  
解题过程：  
深度遍历解决，但是第一次遇到了一点小麻烦，我写的深度遍历里面有个提前返回的情况。我本来想的是如果找到就返回正确，都找不到就返回错误，然而这并不是深度遍历的思路，在我思考过后，我发现返回左边的深度遍历结果或者右边的深度遍历结果才是正确的答案。  
代码：  
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool dfs(TreeNode *node,int targetSum) {
        if (!node) return false;
        if (!node->left && !node->right && node->val == targetSum) return true;
        if (node->left) node->left->val += node->val;
        if (node->right) node->right->val += node->val;
        return dfs(node->left,targetSum) || dfs(node->right,targetSum);
    }

    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        return dfs(root,targetSum);
    }
};
```