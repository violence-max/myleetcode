题目描述：  
![image](/basicaldatastructure/binary_tree/code/isSymertric.md)    
解题过程：  
这个题花了我不少时间，自己是没有做出来哈哈哈，我一开始以为可以分开左右子树进行递归，发现不太行，因为题目问的是二叉树是否轴对称，需要左右两颗子树进行比较而不是单独的左子树或者右子树。然后又想了借助队列来辅助，用一个双向队列，比较每一层的头和尾，实际实现发现这样子做比较困难，因为还要一边入队。也就是说，需要先比较完一层，再入队，但是这个实现我发现至少需要两三个队列，就没继续做了，就直接看题解了。  
题解里面递归的方法是寂寞的，直接拿两个头节点进行比较，还有迭代的方法，用两个队列，但是两个队列放入左右节点的顺序是相反的，真的寂寞。  
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
    bool check(TreeNode* u,TreeNode *v) {
        if (!u && !v) return true;
        if (!u || !v) return false;
        return (u->val == v->val && check(u->left,v->right) && check(u->right,v->left));
    }
    bool isSymmetric(TreeNode* root) {
        return check(root,root);
    }
};
```