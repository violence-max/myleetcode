题目描述：  
![image](/basicaldatastructure/binary_tree/image/image54.png)  
解决过程：  
没做出来  
- dfs：
1. 先序遍历
2. 记录遍历过程中的最小值和最大值
3. 遍历到的每个值和最小值和最大值作差取绝对值，选最大值作为答案  
代码：  
```cpp
class Solution {
public:
    int dfs(TreeNode* root, int mi, int ma) {
        if (!root) return 0;
        int res = max(abs(root->val - mi), abs(root->val - ma));
        mi = min(root->val, mi);
        ma = max(root->val, ma);
        res = max(res, max(dfs(root->left, mi, ma), dfs(root->right, mi, ma)));
        return res;
    }
    int maxAncestorDiff(TreeNode* root) {
        return dfs(root, root->val, root->val);
    }
};
```