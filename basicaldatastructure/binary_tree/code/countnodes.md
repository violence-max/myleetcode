题目描述：  
![image](/basicaldatastructure/binary_tree/image/image17.png)  
解题过程：  
直接深度遍历了，我是天才。  
看了题解，才发现用完全二叉树的性质去做这道题才是正确的。比较让我惊讶的是二叉树的位运算性质，以及用二分法寻找底层节点是否存在的做法，还有二分法求中间节点的写法。  
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
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        int left = countNodes(root->left);
        int right = countNodes(root->right);
        return left + right + 1;
    }
};
```