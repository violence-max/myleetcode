题目描述：  
![image](/basicaldatastructure/binary_tree/image/image34.png)  
解题过程：  
深度遍历加递归查找解决。对于每一个节点我对它能否到达两个节点进行判断，如果可以，并且这个节点的高度是最高的，那么把这个节点记为答案。初始时高度设置为int的最小值。  
看了题解，发现了后序遍历优化的思想，自底向上查找公共节点，强无敌，就是找到了不能提前退出有点麻。还有哈希表存储父节点的想法，也是很好的优化。  
代码：  
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    int min = INT_MIN;
    TreeNode* ancestor = nullptr;
public:
    bool check(TreeNode* start,TreeNode* dst) {
        if (!start) return false;
        if (start->val == dst->val) return true;
        return check(start->left,dst) || check(start->right,dst);
    }

    void dst(TreeNode* root,TreeNode* p,TreeNode* q,int height) {
        if (check(root,p) && check(root,q)) {
            if (height > min) {
                min = height;
                ancestor = root;
            }
        }
        height++;
        if (root->left) dst(root->left,p,q,height);
        if (root->right) dst(root->right,p,q,height);
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dst(root,p,q,0);
        return ancestor;
    }
};
```