题目描述：  
![image](/basicaldatastructure/binary_tree/image/image46.png)  
解决过程：  
这道题我之前居然做过，做到一半不做了，不知道为什么，还用的广度优先搜索和很多个数组，挺二的一个方法。  
2分钟递归ac。但是在做的时候我发现三元运算符的使用还是要谨慎，有时候括号加不好，有可能编译器会认错变量  
代码：  
```cpp
class Solution {
public:
    int sumNumbers(TreeNode* node, TreeNode* parent) {
        if (!node) return 0;
        if (parent) {
            node->val += parent->val * 10;
        }
        if (!node->left && !node->right) {
            return node->val;
        }
        return sumNumbers(node->left,node) + sumNumbers(node->right,node);
    }

    int sumNumbers(TreeNode* root) {
        return sumNumbers(root,nullptr);
    }
};
```