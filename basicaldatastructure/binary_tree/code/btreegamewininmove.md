题目描述：  
![image](/basicaldatastructure/binary_tree/image/image49.png)  
解决过程：
27分钟ac，写得比较久，有点难受，实际上就是统计x节点的父节点数目和左右子节点数目（不准确表达了，大概是这个意思）。题解写得比我好很多啊，包括统计一颗树的节点数目，寻找x节点时向左和向右找的逻辑，以及用n减去x子树的节点数目来获取剩余的节点数目，还有通过判断三个区域的节点数目是否有一个大于等于(n + 1) / 2来获取答案  
代码：（我的→题解）  
```cpp
class Solution {
public:
    int count(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return 1;
        return count(root->left) + 1 + count(root->right);
    }

    TreeNode* searchx(TreeNode* root, int x, int& parent_count) {
        if (!root) return nullptr;
        if (root->val == x) {
            return root;
        }
        ++parent_count;
        TreeNode* left_find = searchx(root->left,x,parent_count);
        TreeNode* right_find = searchx(root->right,x,parent_count);
        if (!left_find && !right_find) return nullptr;
        else if (!left_find) return right_find;
        else return left_find;
    }

    bool btreeGameWinningMove(TreeNode* root, int n, int x) {
        int parent_count = 0;
        TreeNode* x_node = searchx(root,x,parent_count);
        int left_count = count(x_node->left), right_count = count(x_node->right);
        return parent_count > (left_count + right_count) || left_count > (parent_count + right_count) 
               || right_count > (parent_count + left_count);
    }
};
```  
```cpp
class Solution {
public:
    bool btreeGameWinningMove(TreeNode* root, int n, int x) {
        TreeNode* xNode = find(root, x);
        int leftSize = getSubtreeSize(xNode->left);
        if (leftSize >= (n + 1) / 2) {
            return true;
        }
        int rightSize = getSubtreeSize(xNode->right);
        if (rightSize >= (n + 1) / 2) {
            return true;
        }
        int remain = n - 1 - leftSize - rightSize;
        return remain >= (n + 1) / 2;
    }

    TreeNode* find(TreeNode *node, int x) {
        if (node == NULL) {
            return NULL;
        }
        if (node->val == x) {
            return node;
        }
        TreeNode* res = find(node->left, x);
        if (res != NULL) {
            return res;
        } else {
            return find(node->right, x);
        }
    }

    int getSubtreeSize(TreeNode *node) {
        if (node == NULL) {
            return 0;
        }
        return 1 + getSubtreeSize(node->left) + getSubtreeSize(node->right);
    }
};
```