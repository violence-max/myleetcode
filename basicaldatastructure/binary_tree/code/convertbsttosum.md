题目描述：  
![image](/basicaldatastructure/binary_tree/image/image39.png)  
解题过程：  
一开始我想到的就是递归，用一个全局变量数组，先中序遍历把节点存起来，然后反向数组，除了最后一个节点以外，其余节点依次加上后一个节点的值。但是我做的时候发现在数组里面改变的值改变不了root？然后我又改成存储节点的地址，直接操作地址，还是不行？之后想反向中序遍历，但是发现处理不了5这个节点的值，看了题解发现可以设置一个全局变量记录累加值，很秀。最后又重新试了一开始的方法，结果过了。。。无语，不知道第一次什么情况。  
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
    vector<TreeNode*> arr;
public:
    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        arr.push_back(root);
        dfs(root->right);
    }
    TreeNode* convertBST(TreeNode* root) {
        dfs(root);
        for (int i = arr.size()-1; i >= 0; i--) {
            if (i+1 < arr.size()) {
                arr[i]->val += arr[i+1]->val;
            }
        }
        return root;
    }
};
```