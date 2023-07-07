题目描述：

![image](/basicaldatastructure/binary_tree/image/image61.png)

解决过程：

做了三十分钟左右

代码：

```cpp
class Solution {
public:
    unordered_map<int, TreeNode*> m1, m2;
    string getDirections(TreeNode* root, int startValue, int destValue) {
        function<void(TreeNode*, TreeNode*)> dfs = [&](TreeNode* h, TreeNode* p) -> void {
            if (!h) return;
            int x = h->val;
            m1[x] = h;
            m2[x] = p;
            dfs(h->left, h);
            dfs(h->right, h);
        };
        dfs(root, nullptr);
        string ans;
        function<bool(TreeNode*, TreeNode*)> find = [&](TreeNode* cur, TreeNode* l) -> bool {
            if (cur && cur->val == destValue) {
                return true;
            }
            
            // cout << "find : " << cur->val << endl;
            if (m2.count(cur->val) && m2[cur->val] && m2[cur->val] != l) {
                ans += 'U';
                if (find(m2[cur->val], cur)) return true;
                ans.pop_back();
            }
            if (cur->left && cur->left != l) {
                ans += 'L';
                if (find(cur->left, cur)) return true;
                ans.pop_back();
            }
            if (cur->right && cur->right != l) {
                ans += 'R';
                if (find(cur->right, cur)) return true;
                ans.pop_back();
            }
            return false;
        };
        find(m1[startValue], nullptr);
        return ans;
    }
};
```

```cpp
class Solution {
public:
    string getDirections(TreeNode* root, int startValue, int destValue) {
        unordered_map<TreeNode*, TreeNode*> fa;   // 父节点哈希表
        TreeNode* s = nullptr;   // 起点节点
        TreeNode* t = nullptr;   // 终点节点
        
        // 深度优先搜索维护哈希表与起点终点
        function<void(TreeNode*)> dfs = [&](TreeNode* curr) {
            if (curr->val == startValue) {
                s = curr;
            }
            if (curr->val == destValue) {
                t = curr;
            }
            if (curr->left) {
                fa[curr->left] = curr;
                dfs(curr->left);
            }
            if (curr->right) {
                fa[curr->right] = curr;
                dfs(curr->right);
            }
        };
        
        dfs(root);
        
        // 求出根节点到对应节点的路径字符串
        function<string(TreeNode*)> path = [&](TreeNode* curr) {
            string res;
            while (curr != root) {
                TreeNode* par = fa[curr];
                if (curr == par->left) {
                    res.push_back('L');
                }
                else {
                    res.push_back('R');
                }
                curr = par;
            }
            reverse(res.begin(), res.end());
            return res;
        };
        
        string path1 = path(s);
        string path2 = path(t);
        // 计算最长公共前缀并删去对应部分，同时将 path_1 逐字符修改为 'U'
        int l1 = path1.size(), l2 = path2.size();
        int i = 0;
        while (i < min(l1, l2)) {
            if (path1[i] == path2[i]) {
                ++i;
            }
            else {
                break;
            }
        }
        string finalpath(l1 - i, 'U');   // 最短路径对应字符串 
        finalpath.append(path2.substr(i, l2 - i));
        return finalpath;
    }
};
```