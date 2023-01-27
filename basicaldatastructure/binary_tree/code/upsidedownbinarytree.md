题目描述：  
![image](/basicaldatastructure/binary_tree/image/image47.png)  
解决过程：  
第一道premium的题目，居然没有官方题解，不同看哪里了以后  
很简单，就是前序遍历而已，具体见代码  
代码：  
```cpp
class Solution {
public:
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
        if (!root || !root->left) return root;
        TreeNode* right = root->right, * left = root->left;
        TreeNode* newroot = upsideDownBinaryTree(left);
        root->left = nullptr;
        root->right = nullptr;
        left->right = root;
        left->left = right;
        return newroot;
    }
};
```