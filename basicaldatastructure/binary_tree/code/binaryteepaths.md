题目描述：  
![image](/basicaldatastructure/binary_tree/code/binaryteepaths.md)  
解题过程：  
这loser的人生我是一点都不想过下去了  
题解说是回溯，我也不知道这个题是回溯，总而言之就是感觉用一个递归就可以写，然后就开始尝试，一开始先把头节点的值加入字符串中，如果左孩子存在则深度遍历左孩子，如果右孩子存在则深度遍历右孩子。但是我发现我最后忽略了只有头节点的情况。  
真的loser  
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
    void construct(TreeNode *node,string str,vector<string> &res) {
        str += "->";
        str += to_string(node->val);
        if (!node->left && !node->right) res.push_back(str);
        if (node->left) construct(node->left,str,res);
        if (node->right) construct(node->right,str,res);
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        if (!root) return {};
        string str;
        vector<string> res;
        str += to_string(root->val); 
        if (!root->left && !root->right) res.push_back(str);
        if (root->left) construct(root->left,str,res);
        if (root->right) construct(root->right,str,res);
        return res;
    }
};
```