题目描述：  
![image](/basicaldatastructure/binary_tree/image/image29.png)  
解题过程：  
一开始还以为这应该是个medium才对啊，一用递归，这逻辑也太简单了  
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
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 && root2) {
            root1->val += root2->val;
            root1->left = mergeTrees(root1->left,root2->left);
            root1->right = mergeTrees(root1->right,root2->right);
            return root1;
        }
        if (root1) return root1;
        if (root2) return root2;
        else return nullptr;
    }
};
```