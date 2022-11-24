题目描述：  
![image](/basicaldatastructure/binary_tree/image/image27.png)  
解题过程：  
仿照从后序和中序遍历构建二叉树直接秒杀，但是出现了很傻逼的一幕，我在判断前序遍历数组是否非空的时候用了前序遍历数组的大小减去1再加一个感叹号进行判断，实际上不应该减一的，然后直接返回空指针。因为感叹号的优先级大于减号，所以会出现1-1 = 0然后返回空指针的情况。  
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
private:
    int preidx = 0;
    unordered_map<int,int> in;
public:
    TreeNode *recur(int left,int right,vector<int> preorder,vector<int> inorder) {
        if (left > right) return nullptr;
        TreeNode *root = new TreeNode(preorder[preidx++]);
        int idx = in[root->val];
        root->left = recur(left,idx - 1,preorder,inorder);
        root->right = recur(idx + 1,right,preorder,inorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (!(int)preorder.size()) return nullptr;
        int idx = 0;
        for (auto val : inorder) {
            in[val] = idx++; 
        }
        return recur(0,(int)inorder.size()-1,preorder,inorder); 
    }
};
```