题目描述：  
![image](/basicaldatastructure/binary_tree/image/image13.png)  
解题过程：  
我用的是深度遍历，但是看了题解发现可以直接递归  
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
        if (root->left) leftLevel =  dfs(root->left,level+1);
        if (root->right) rightLevel = dfs(root->right,level+1);
        return (leftLevel > rightLevel) ? leftLevel : rightLevel; 
    }
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        return dfs(root,1);
    }
};
```