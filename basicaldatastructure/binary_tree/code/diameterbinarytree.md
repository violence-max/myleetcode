题目描述：  
![image](/basicaldatastructure/binary_tree/image/image53.png)  
解决过程：  
写了一个双重递归，很慢，写双重递归的原因是忽略了最长路径不一定经过根节点  
看了题解，其实一次递归就可以了，重点在于维护ans  
代码：（双重递归→一遍过）  
```cpp
class Solution {
private:
    unordered_map<TreeNode*,int> map;
public:
    int dfs(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return 1;
        if (map.count(root)) return map[root];
        return map[root] = 1 + max(dfs(root->left),dfs(root->right));
    }

    int diameterOfBinaryTree(TreeNode* root) {
        if (!root) return 0;
        int ret =  dfs(root->left) + dfs(root->right);
        ret = max(ret,diameterOfBinaryTree(root->left));
        ret = max(ret,diameterOfBinaryTree(root->right));
        return ret;
    }
};
```  
```cpp
class Solution {
int ans;
public:
    int dfs(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return 1;
        int leftHight = dfs(root->left);
        int rightHight = dfs(root->right);
        ans = max(ans,leftHight + rightHight + 1);
        return max(leftHight,rightHight) + 1; 
    }

    int diameterOfBinaryTree(TreeNode* root) {
        if (!root) return 0;
        ans = 1;
        dfs(root);
        return ans - 1;
    }
};
```