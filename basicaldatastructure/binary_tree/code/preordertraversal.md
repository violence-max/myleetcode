题目描述：  
![image](/basicaldatastructure/binary_tree/image/image2.png)  
经典解法，需要关注的是题解中的迭代法和莫里斯遍历，莫里斯遍历可以让空间复杂度降到O(1)，但是我没有研究。  
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
    static void preorder(TreeNode *root,vector<int> &res) {
        if (!root) return;
        res.push_back(root->val);
        preorder(root->left,res);
        preorder(root->right,res);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(root,res);
        return res;
    }
};
```