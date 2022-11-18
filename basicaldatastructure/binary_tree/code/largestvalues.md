题目描述：  
![image](/basicaldatastructure/binary_tree/image/image10.png)  
解题过程：  
无脑  
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
    vector<int> largestValues(TreeNode* root) {
        if (!root) return {};
        queue<TreeNode*> q;
        q.push(root);
        vector<int> res;
        while (!q.empty()) {
            int count = 0, size = q.size();
            int maxN = INT_MIN;
            while (count++ < size) {
                TreeNode *node = q.front(); q.pop();
                if (node->val > maxN) maxN = node->val;
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            res.push_back(maxN);
        }
        return res;
    }
};
```