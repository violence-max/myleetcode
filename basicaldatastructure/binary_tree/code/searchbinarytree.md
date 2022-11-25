题目描述：  
![image](/basicaldatastructure/binary_tree/image/image30.png)  
解题过程：  
二叉树的搜索，用屁股想都能想出来  
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
    TreeNode* searchBST(TreeNode* root, int val) {
        if (!root) return nullptr;
        if (root->val == val) return root;
        else {
            if (val < root->val) return searchBST(root->left,val);
            else return searchBST(root->right,val);
        }
    }
};
```