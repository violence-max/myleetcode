题目描述：  
![image](/basicaldatastructure/binary_tree/image/image21.png)    
解题过程：  
在做这题之前回去复习了一下树的子结构，发现那个题是寂寞的。看了这个题的题解，这个题和那个题的不同就是在于子树和子结构，子结构可以是树的任意一个部分，子树是必须包含叶子节点的。也就是说，完全可以使用深度遍历递归就可以了。  
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
    bool check(TreeNode *root,TreeNode *subRoot) {
        if (!root && !subRoot) return true;
        if (!root || !subRoot || root->val != subRoot->val) return false;
        return check(root->left,subRoot->left) && check(root->right,subRoot->right);
    }
    bool dfs(TreeNode *root,TreeNode  *subRoot) {
        if (!root) return false;
        return check(root,subRoot) || dfs(root->left,subRoot) || dfs(root->right,subRoot);
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        return dfs(root,subRoot);
    }
};
```