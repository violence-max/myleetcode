题目描述：  
![image](/basicaldatastructure/binary_tree/image/image18.png)  
解题过程：  
本来想用高度解的，但是发现需要考虑每个节点的高度，觉得时间复杂度太高，就换了一种思路。用单独判断一个节点是否平衡来解决，以为如果一个节点只有左右节点必有一个不存在并且高度之差超过1才不平衡，实际上可以左右节点都存在但是高度之差也超过1。然后我就又回到了高度解，虽然求出每个节点的高度比较暴力。。。  
看了题解，发现了后序遍历的优化方法，贼牛逼。是一种及时止损的方法，从后往前计算高度，如果发现了高度之差不符合要求就直接返回-1。  
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
    int hight(TreeNode* root) {
        if (!root) return 0;
        int left = 0,right = 0;
        if (root->left) left = hight(root->left);
        if (root->right) right = hight(root->right);
        return max(left,right) + 1;
    }
    bool isBalanced(TreeNode* root) {
        if (!root) return true;
        int left = hight(root->left);
        int right = hight(root->right);
        return ((left - right >= -1) && (left - right <= 1) && isBalanced(root->left) && isBalanced(root->right));
    }
};
```