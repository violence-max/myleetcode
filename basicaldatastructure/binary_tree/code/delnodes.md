题目描述：  
![image](/basicaldatastructure/binary_tree/image/image55.png)  
解题过程：  
没做出来  
- 深度优先搜索：dfs(Treenode* node, bool is_root) → Treenode*  
is_root：因为在遍历的过程中有可能遇到要被删除的节点，这个被删除的节点的子节点是会成为森林中的新的根节点之一的，因此这时候is_root为true  
代码：（dfs）  
```cpp
class Solution {
public:
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> set_ (to_delete.begin(), to_delete.end());
        vector<TreeNode*> ans;

        function<TreeNode* (TreeNode* , bool)> dfs = [&](TreeNode* node, bool is_root) -> TreeNode* {
            if (!node) return nullptr;

            bool deleted = set_.count(node->val) ? true : false;
            // 更改指针指向
            // 如果左节点或者右节点被删除则更新为nullptr，否则保持不变
            // 如果node节点本身就要被删除，则给左节点和又节点传达“你们是潜在的新节点的信息”
            node->left = dfs(node->left, deleted);
            node->right = dfs(node->right, deleted);
            if (deleted) {
                return nullptr;
            } else {
                if (is_root) {
                    ans.push_back(node);
                }
                return node;
            }
        };

        dfs(root, true);
        return ans;
    }
};
```