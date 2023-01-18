题目描述：  
![image](/basicaldatastructure/binary_tree/image/image43.png)  
解决过程：  
十分钟ac，但是过程出现了一个问题，因为有负数的存在所以不能提前剪枝。题解还给出了一种广度优先遍历解决的方法，但是这种方法需要用一个map来记录父节点，代码部分会记录一下  
深度优先遍历的时候可以试着把sum写道dfs函数参数列表里面  
代码：（深度优先→广度优先）  
```cpp
class Solution {
private:
    int targetSum,sum = 0;
    vector<vector<int>> ans;
    vector<int> tmp;
public:
    void dfs(TreeNode* root) {
        tmp.push_back(root->val);
        sum += root->val;
        if (sum == targetSum && !root->left && !root->right) {
            ans.push_back(tmp);
        }
        if (root->left) dfs(root->left);
        if (root->right) dfs(root->right);
        sum -= root->val;
        tmp.pop_back();
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if (!root) return ans;
        this->targetSum = targetSum;
        dfs(root);
        return ans;
    }
};
```
```cpp
class Solution {
public:
    vector<vector<int>> ret;
    unordered_map<TreeNode*, TreeNode*> parent;

    void getPath(TreeNode* node) {
        vector<int> tmp;
        while (node != nullptr) {
            tmp.emplace_back(node->val);
            node = parent[node];
        }
        reverse(tmp.begin(), tmp.end());
        ret.emplace_back(tmp);
    }

    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) {
            return ret;
        }

        queue<TreeNode*> que_node;
        queue<int> que_sum;
        que_node.emplace(root);
        que_sum.emplace(0);

        while (!que_node.empty()) {
            TreeNode* node = que_node.front();
            que_node.pop();
            int rec = que_sum.front() + node->val;
            que_sum.pop();

            if (node->left == nullptr && node->right == nullptr) {
                if (rec == targetSum) {
                    getPath(node);
                }
            } else {
                if (node->left != nullptr) {
                    parent[node->left] = node;
                    que_node.emplace(node->left);
                    que_sum.emplace(rec);
                }
                if (node->right != nullptr) {
                    parent[node->right] = node;
                    que_node.emplace(node->right);
                    que_sum.emplace(rec);
                }
            }
        }

        return ret;
    }
};
```