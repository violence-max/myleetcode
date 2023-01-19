题目描述：  
![image](/basicaldatastructure/binary_tree/image/image44.png)  
解题过程：  
瞬间ac了。但是在改变指针方向的过程中，改变了根节点的右指针忘记一起更改左指针了，导致左右指针指向同一节点，一开始没注意到  
代码：  
```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        if (!root) return;
        if (!root->right && !root->left) return;
        TreeNode* right = root->right,* left = root->left;
        root->right = root->left;
        root->left = nullptr;
        flatten(left);
        TreeNode* tmp = root;
        while (tmp && tmp->right)
            tmp = tmp->right;
        tmp->right = right;
        flatten(right);
    }
};
```