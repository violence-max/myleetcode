题目描述：  
![image](/basicaldatastructure/binary_tree/image/image48.png)  
解决过程：  
用的是红黑树的节点移动的方法，很慢，题解给出了用数组直接存储和用栈的方法来解决  
代码：（节点移动→栈）  
```cpp
class BSTIterator {
private:
    TreeNode* _root;
    TreeNode* _next = nullptr;
    unordered_map<TreeNode*,TreeNode*> map;
    TreeNode* mostright;
public:
    BSTIterator(TreeNode* root) {
        _root = root;
        findparent(_root,nullptr);
        while (root->right) {
            root = root->right;
        }
        mostright = root;
    }

    TreeNode* mostleft(TreeNode* root) {
        if (!root) return nullptr;
        while (root->left) {
            root = root->left;
        }
        return root;
    }   

    void findparent(TreeNode* node, TreeNode* parent) {
        if (!node) return;
        map[node] = parent;
        findparent(node->left,node);
        findparent(node->right,node);
    }

    int next() {
        if (!_next) {
            _next = mostleft(_root);
        } else {
            if (_next->right) {
                _next = mostleft(_next->right);
            } else {
                while (_next != map[_next]->left) {
                    _next = map[_next];
                }
                _next = map[_next];
            }
        }
        return _next->val;
    }
    
    bool hasNext() {
        return _next != mostright;
    }
};
```
```cpp
class BSTIterator {
private:
    TreeNode* cur;
    stack<TreeNode*> stk;
public:
    BSTIterator(TreeNode* root): cur(root) {}
    
    int next() {
        while (cur != nullptr) {
            stk.push(cur);
            cur = cur->left;
        }
        cur = stk.top();
        stk.pop();
        int ret = cur->val;
        cur = cur->right;
        return ret;
    }
    
    bool hasNext() {
        return cur != nullptr || !stk.empty();
    }
};
```