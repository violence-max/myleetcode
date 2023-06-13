题目描述：

![image](/basicaldatastructure/binary_tree/image/image60.png)

解决过程：

写了两个半小时，终于熬出来了

代码：（深度优先搜索→题解）

```cpp
class Solution {
public:
    unordered_map<TreeNode*, int> h;
    unordered_map<int, TreeNode*> int2t;
    TreeNode* dfs(const string& traversal, int hight, int& index, int length) {
        if (index >= length) {
            return nullptr;
        }

        TreeNode* node = get(traversal, index, length, hight);
        int origin = index;
        while (index < length && traversal[index] == '-') {
            index++;
        }
        int nexthight = index - origin;
        if (nexthight <= hight) {
            cout << "before we get the tree, the order is trace back" << endl;
            TreeNode* strange = get(traversal, index, length, nexthight);
            index = origin + nexthight;
            return node;
        }

        TreeNode* ltree = dfs(traversal, nexthight, index, length);
        node->left = ltree;
        cout << "the tree" << node->val << " 's left sun is " << ltree->val << endl;
        if (int2t.count(index) && h[int2t[index]] != nexthight) {
            return node;
        }
        TreeNode* rtree = dfs(traversal, nexthight, index, length);
        node->right = rtree;
        cout << "the tree" << node->val << " 's right sun is " << rtree->val << endl;
        return node;
    }

    TreeNode* get(const string& traversal, int& index, int length, int height) {
        const int origin = index;
        if (int2t.count(index)) {
            while(index < length && traversal[index] != '-') {
                index++;
            }
            return int2t[origin];
        }

        int val = 0;
        while (index < length && traversal[index] != '-') {
            val = val * 10 + traversal[index] - '0';
            index++;
        }
        TreeNode* node = new TreeNode(val);
        cout << "Now we get a tree, and its value is:" << ' ' << node->val << endl;
        int2t[origin] = node;
        h[node] = height;
        return node;
    }

    TreeNode* recoverFromPreorder(string traversal) {
        int n = traversal.size();
        int i = 0;
        return dfs(traversal, 0, i, n);
    }
};
```

```cpp
class Solution {
public:
    TreeNode* recoverFromPreorder(string traversal) {
        stack<TreeNode*> path;
        int pos = 0;
        while (pos < traversal.size()) {
            int level = 0;
            while (traversal[pos] == '-') {
                ++level;
                ++pos;
            }
            int value = 0;
            while (pos < traversal.size() && isdigit(traversal[pos])) {
                value = value * 10 + (traversal[pos] - '0');
                ++pos;
            }
            TreeNode* node = new TreeNode(value);
            if (level == path.size()) {
                if (!path.empty()) {
                    path.top()->left = node;
                }
            }
            else {
                while (level != path.size()) {
                    path.pop();
                }
                path.top()->right = node;
            }
            path.push(node);
        }
        while (path.size() > 1) {
            path.pop();
        }
        return path.top();
    }
};
```