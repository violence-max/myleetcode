题目描述：  
![image](/algorithmn/dynamic_programming/image/image20.png)  
解题过程：  
这就是记忆化搜索！！什么动态规划？去tm的，二叉树还动态规划呢，遍历和递归才是王道，直接从上往下，考虑选取当前节点与不选取当前节点时的能得到的最大盗窃金额，一计算出一个节点的最大金额即存入哈希map中，防止多次计算，强无敌！  
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
    unordered_map<TreeNode*,int> map;
public:
    int excuteleftrightsum(TreeNode* root,int sum) {
        if (root) {
            sum = traval(root);
            map[root] = sum;
        }
        return sum;
    }

    int excuteleftrightchild(TreeNode* root,int sum) {
        if (map.find(root) != map.end()) {
            sum = map[root];
        }
        return sum;
    }

    int traval(TreeNode* root) {
        if (!root->left && !root->right) return root->val;
        else {
            int leftsum = 0,rightsum = 0,leftchild = 0,rightchild = 0;
            leftsum = excuteleftrightsum(root->left,leftsum);
            rightsum = excuteleftrightsum(root->right,rightsum);
            if (root->left) leftchild = excuteleftrightchild(root->left->left,leftchild) + excuteleftrightchild(root->left->right,leftchild);
            if (root->right) rightchild = excuteleftrightchild(root->right->left,rightchild) + excuteleftrightchild(root->right->right,rightchild);
            return max(leftsum+rightsum,root->val+leftchild+rightchild);
        }
    }

    int rob(TreeNode* root) {
        return traval(root);
    }
};
```