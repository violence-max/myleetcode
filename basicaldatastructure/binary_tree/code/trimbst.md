题目描述：  
![image](/basicaldatastructure/binary_tree/image/image37.png)  
解题过程：  
深度遍历所有节点然后遇到范围不对的就删除哈哈哈，机智如我。但是官方并没有准备这一种解法的正确答案，反正经过我的验证还是能得到一颗二叉搜索树，只不过高度有点高。  
看了题解，尤其是那个迭代的方法，把二叉搜索树的搜索的特性发挥到了极致。如果一个节点值小于low，则丢掉这个节点和这个节点的左子树。如果一个节点值大于high，则丢掉这个节点和这个节点的右子树。我们先处理头节点，然后处理头节点的左子树。处理左子树时，因为左子树中所有的值都要小于头节点的值，而头节点的值小于high，因此左孩子的右子树是正确的子树结构。所以只需要处理左孩子的左子树就好了。头节点的右子树同理。  
递归也很寂寞。  
代码：  
暴力：  
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
    TreeNode* deleteNode(TreeNode* root,int val) {
        if (!root) return nullptr;
        if (root->val > val) root->left = deleteNode(root->left,val);
        else if (root->val < val) root->right = deleteNode(root->right,val);
        else if (root->val == val) {
            if (!root->left && !root->right) return nullptr;
            if (!root->left) return root->right;
            if (!root->right) return root->left;
            TreeNode *successor = root->right;
            while (successor->left) successor = successor->left;
            root->right = deleteNode(root->right,successor->val);
            successor->right = root->right;
            successor->left = root->left;
            return successor;
        }
        return root;
    }
    
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;
        if (root->val <= high && root->val >= low) {
            root->left = trimBST(root->left,low,high);
            root->right = trimBST(root->right,low,high);
        } else {
            root = deleteNode(root,root->val);
            root = trimBST(root,low,high);
        }
        return root;
    }
};
```  
递归：  
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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;
        if (root->val < low) root = trimBST(root->right,low,high);
        else if (root->val > high) root = trimBST(root->left,low,high);
        else {
            root->left = trimBST(root->left,low,high);
            root->right = trimBST(root->right,low,high);
        }
        return root;
    }
};
```