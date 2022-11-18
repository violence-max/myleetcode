题目描述：  
![image](/basicaldatastructure/binary_tree/image/image7.png)  
解题过程：  
二叉树的右视图一开始不知道是什么东西，我以为就是层序遍历加一个限定条件：只允许出现右边的节点，没有想到是站在右边看到的第一个节点。  
看了题解以后我才对这个题目恍然大悟知道它在讲什么。然后改了一下自己的代码，用了一个哈希map来存储每一层最后一个节点的值，最后加到答案数组里。题解里面深度优先的方法是寂寞的。  
代码：  
```jsx
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
    vector<int> rightSideView(TreeNode* root) {
        if (!root) return {};
        queue<TreeNode*> q;
        q.push(root);
        vector<int> res;
        int level = 0;
        unordered_map<int,int> map;
        while (!q.empty()) {
            int count = 0,size = q.size();
            while (count++ < size) {
                TreeNode *node = q.front(); q.pop();
                map[level] = node->val;
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            level++;
        }
        for (int i = 0; i < level; i++) {
            res.push_back(map[i]);
        }
        return res;
    }
};
```