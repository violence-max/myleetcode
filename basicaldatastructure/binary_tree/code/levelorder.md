题目描述：  
![image](/basicaldatastructure/binary_tree/image/image5.png)  
解题过程：  
经典的层序遍历，当然用广度优先啊。思路其实很简单，但是实现起来居然花了快两个小时。。。自己的coding能力确实很差，还需要多练哈。踩了很多坑哈哈哈，都被我用肉眼debug法解决了，几次想用gdb来着都恰好解决了。  
值得说的是自己的写法和题解有什么不同：题解的当然更加简洁，我用的数据结构是双向队列，但实际上，用队列就可以哈哈哈。  
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
    void excute(deque<TreeNode*> &q,vector<int> &res,TreeNode* node,int &tempCount) {
        if (node) {
            tempCount += 1;
            res.push_back(node->val);
            q.push_front(node);
        }
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) return {};
        deque<TreeNode*> q;
        q.push_back(root);
        vector<vector<int>> res = {{root->val}};
        int count = 1;
        while (!q.empty()) {
            int i = 0,tempCount = 0;
            vector<int> temp;
            while (!q.empty() && i++ < count) {
                TreeNode *tempNode = q.back();
                if (tempNode) {
                    excute(q,temp,tempNode->left,tempCount);
                    excute(q,temp,tempNode->right,tempCount);
                }
                q.pop_back();
            }
            count = tempCount;
            if (temp.size()) {
                res.push_back(temp);
            } else {
                break;
            }
        }
        return res;
    }
};
```