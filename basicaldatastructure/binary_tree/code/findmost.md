题目描述：  
![image](/basicaldatastructure/binary_tree/image/image33.png)  
解题过程：  
我用的哈希表，第一次先记录每个值出现的次数，然后扫描一遍哈希表得到众数，再扫描一遍哈希表得到具体值。  
看了题解，优化解主要是不使用哈希表，有一个更新函数，虽然我觉得还不如使用哈希表，因为更新函数的O(1)语句显然多余哈希表。最优化的化石莫里斯中序遍历，空间复杂度O(1)，但是暂时还没有研究，先放着吧。  
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
    void dfs(TreeNode* root,unordered_map<int,int>& most) {
        if (!root) return;
        most[root->val]++;
        dfs(root->left,most);
        dfs(root->right,most);
    }
    vector<int> findMode(TreeNode* root) {
        unordered_map<int,int> most;
        dfs(root,most);
        int max = INT_MIN;
        vector<int> ans;
        for (auto [val,count] : most) {
            if (max < count) {
                max = count;
            }
        }
        for (auto [val,count] : most) {
            if (count == max) {
                ans.push_back(val);
            }
        }
        return ans;
    }
};
```