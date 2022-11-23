题目描述：  
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0b173912-3428-445f-92ec-2aacfbc5735b/Untitled.png)  
解题过程：  
一开始以为是求所有的左孩子的值之和，就没过，然后看到了左叶子，就加了个叶子节点的判断条件，但是还是没过！！！定睛一看，哎，if后面直接加了个冒号，条件语句没有进去，嗨嗨嗨。  
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
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) return 0;
        int sum = 0;
        if (root->left) {
            if (!root->left->left && !root->left->right)
                sum += root->left->val;
        }
        sum += sumOfLeftLeaves(root->left);
        sum += sumOfLeftLeaves(root->right);
        return sum;
    }
};
```