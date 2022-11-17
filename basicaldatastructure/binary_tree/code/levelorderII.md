题目描述：  
![image](/basicaldatastructure/stackandquere/image/image10.png)  
解题过程：  
就是层序遍历而已，区别就是输出是从叶子那一层到根节点，我写的是vector从头插入的方法，但是这种方法开销比较大，很慢。看了题解，用的是反转，相当于先从后插入，然后再反转。  
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        if (!root) return {};
        queue<TreeNode*> q;
        q.push(root);
        vector<vector<int>> res;
        while (!q.empty()) {
            vector<int> temp;
            int count = 0,size = q.size();
            while (count++ < size) {
                TreeNode *node = q.front(); q.pop();
                temp.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            res.insert(res.begin(),temp);
        }
        return res;
    }
};