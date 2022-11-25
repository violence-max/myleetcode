题目描述：  
![image](/basicaldatastructure/binary_tree/image/image32.png)  
解题过程：  
太困了，不然肯定一下子就秒杀了，硬是拖了快一个小时，中间睡了一觉加吃了四个鸡蛋。  
我用的是中序遍历利用升序的性质来解决。做的时候出了个问题，一个是以为会出现只有一个节点的情况，就出现了初始的差值设置成最左下角节点的情况。  
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
    int getMinimumDifference(TreeNode* root) {
        int min = INT_MIN,ans = INT_MAX;
        stack<TreeNode*> stk;
        while (!stk.empty() || root) {
            while (root) {
                stk.push(root);
                root = root->left;
            }
            root = stk.top(); stk.pop();
            if (min == INT_MIN) {
                min = root->val;
                root = root->right;
                continue;
            }
            int res = (root->val - min >= 0) ? (root->val - min) : (min - root->val);
            ans = (res < ans) ? res : ans;
            min = root->val;
            root = root->right;
        }
        return ans;
    }
};
```