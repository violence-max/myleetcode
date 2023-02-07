题目描述：  
![image](/basicaldatastructure/binary_tree/image/image50.png)  
解决过程：  
1分钟ac  
代码：  
```cpp
class Solution {
public:
    bool evaluateTree(TreeNode* root) {
        if (!root->left & !root->right) return root->val;
        int _left = evaluateTree(root->left);
        int _right = evaluateTree(root->right);
        return root->val == 2 ? _left | _right : _left & _right;
    }
};
```