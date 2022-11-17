题目描述：  
![image](/basicaldatastructure/binary_tree/image/image6.png)  
解题过程：  
测评题目，花了五十多分钟没有做出来，后来又花了一个半小时来debug，大部分时间在肉眼debug，肉眼debug一直认为自己是对的，实则不然，我用了gdb之后光速知道自己错在哪，然后一改就通过了。  
我的想法是用广度优先搜素，一开始，如果根节点为空，则返回0，否则将根节点入队，并且将其值加入权重数组中，权重数组是类似层序遍历的一个二维数组，同时设置一个flag数组，如果有节点为子结点，即既无左孩子也无右孩子则将对应处设置为true，否则设置为false，经过完整的层次遍历之后，通过flag数组来判断哪个值为叶子节点的权重，最终加入答案中。  
这里自己写的一个最大的失误的地方是在处理权重这一块，我以为每个节点的权重应该是上一个节点的权重乘以10再加上当前节点的值，是这样没错，但是我每次处理结束都没有更新节点的权重，所以最后错了，哎，三个面试题只做出来一个，惭愧啊。  
看了题解，深度优先搜索的方法太寂寞了，无敌。  
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
    void excute(queue<TreeNode*> &q,vector<int> &tempWeight,TreeNode *child,TreeNode *parent) {
        if (child) {
            q.push(child);
            int weight = parent->val * 10 + child->val;
            tempWeight.push_back(weight);
            child->val = weight;
        }
    }
    int sumNumbers(TreeNode* root) {
        if (!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        vector<vector<int>> weight = {{root->val}};
        vector<vector<bool>> flag;
        int result = 0;
        while (!q.empty()) {
            int count = 0,size = q.size();
            vector<int> tempWeight;
            vector<bool> tempFlag;
            while (count++ < size) {
                TreeNode* temp = q.front(); q.pop();
                if (!temp->left && !temp->right) {
                    tempFlag.push_back(true);
                } else {
                    tempFlag.push_back(false);
                }
                excute(q,tempWeight,temp->left,temp);
                excute(q,tempWeight,temp->right,temp);
            }
            flag.push_back(tempFlag);
            if (tempWeight.size()) weight.push_back(tempWeight);
        }
        int m = flag.size();
        for (int i = 0; i < m; i++) {
            vector<bool> f = flag[i];
            int n = f.size();
            for (int j = 0; j < n; j++) {
                if (f[j]) result += weight[i][j];
            }
        }
        return result;
    }
};
```