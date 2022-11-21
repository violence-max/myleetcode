题目描述：  
![image](/basicaldatastructure/binary_tree/image/image14.png)  
解题过程：  
一开始想用简单递归来解，但是试了一下不太行，就用了深度遍历，虽然也是一种递归哈哈哈。 
题解写得还行，解决了空节点和叶子节点两种基本情况，然后不断取深度最小值  
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
    int dfs(TreeNode *root,int level) {
        if (!root->left && !root->right) return level;
        int leftLevel,rightLevel;
        if (root->left) leftLevel = dfs(root->left,level+1);
        else leftLevel = INT_MAX;
        if (root->right) rightLevel = dfs(root->right,level+1);
        else rightLevel = INT_MAX;
        return (leftLevel < rightLevel) ? leftLevel : rightLevel;
    }
    int minDepth(TreeNode* root) {
        if (!root) return 0;
        return dfs(root,1);
    }
};
```