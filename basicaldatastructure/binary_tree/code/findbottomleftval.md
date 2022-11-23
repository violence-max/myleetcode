题目描述：  
![image](/basicaldatastructure/binary_tree/image/image24.png)  
解题过程：  
用的广度优先搜索，一开始想的是将所有左节点的值从后插入一个数组中，这样一次遍历结束之后这个数组的最后一个节点的值就是返回值了。但是我发现。当只有一个节点和只有两个节点并且没有左孩子的情况是通过不了测试的，然后我就改了。我改成在每一层的循环中将叶子节点从后面插入一个数组中，每次循环取第一个节点作为所求的节点，最后返回该节点的值，虽然这样子有点浪费空间和时间，会有很多无用的装填和赋值过程。最后解决。  
看了题解，广度优先搜索的解法里是从右到左遍历，每次进行节点的值的取值操作，渠道最后自然就是最底层最左节点。深度优先搜索就有点寂寞了，是一个后序遍历，先左后右再根，如果出现了超过已经记录了的高度的高度的节点再进行新的赋值，相当寂寞。  
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
    int findBottomLeftValue(TreeNode* root) {
        if (!root->left && !root->right) return root->val;
        queue<TreeNode*> q;
        q.push(root);
        TreeNode *ans;
        while (!q.empty()) {
            int count = 0,size = q.size();
            vector<TreeNode*> res;
            while (count++ < size) {
                TreeNode *node = q.front(); q.pop();
                if (!node->left && !node->right) res.push_back(node);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            if (res.size()) ans = res.at(0);
        }
        return ans->val;
    }
};
```