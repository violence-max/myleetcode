题目描述：  
![image](/basicaldatastructure/binary_tree/image/image8.png)  
解题过程：  
很经典的一个广度优先搜索策略，看了题解之后发现深度优先搜索是真的寂寞。  
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
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        vector<double> res;
        while (!q.empty()) {
            int count = 0,size = q.size();
            long sum = 0;
            while (count++ < size) {
                TreeNode* node = q.front(); q.pop();
                sum += (long)node->val;
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            double ave = (double) sum / size;
            res.push_back(ave);
        }
        return res;
    }
};
```