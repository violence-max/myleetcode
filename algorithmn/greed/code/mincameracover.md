题目描述：  
![image](/algorithmn/greed/image/image17.png)  
解题过程：  
这个题经历了好多过程：  
1. 把大体思路思考出来，花了半个小时左右，然后开始尝试思路。思路大概就是递归，深度优先搜索，先尝试到达倒数第二层，即一个有左叶子节点或者右叶子节点的节点，并且把这个节点标记为1，总数加1。在回溯的过程，如果叶子节点都没有1则自身设置为1，总数加1。但是后来发现了在左节点或者右节点为叶子节点但是另一边为子树的情况下，只是总数加1，无法进行后续标记（有点忘记了，不知道是不是应该这样表述）
2. 更改了回溯过程中若遍历左或者右的时候对应孩子节点值不为1或者存在另外的一边的叶子节点则将改节点设置为1。然后发现了更贪心的例子→标记前一个节点
3. 单独写了一个dfs函数，其参数中包含了父节点。当遍历的位置的节点不为1时，若还有父节点，则将父节点的值设置成1，但是总数不加1，直到回到父节点并且检测到自身值为1时总数再加1。发现了一条链式二叉树并且总节点数只有4个时的反例，此时对于根节点既没有父节点又没有另一边的孩子，无法标记，直接返回，总数比答案少1。由于这个想法的优先级是面对遍历的那一部分的节点的值有没有设置成1，所以本来就有很大的弊端—回到根节点并且另一边为叶子节点。所以在判断条件上应该多加一个：或者存在另一边的孩子。然后判断孩子节点或者父节点的存在。如果只有父节点那当然标记父节点，但是，如果存在孩子，则必先考虑本节点—父节点的标记只能做到向上贪心而包含不到孩子。
4. 3的结论推导部分  
至此，大体的做题思路应该是：向下递归，先找出基本情况并且将值设置为1，然后考虑左右子树存在与否的三种可能（都不存在的可能被pass掉了），存在则遍历，如果检测到自身值为1（由下层设置），则总数加1并且返回，如果遍历的部分不等于1或者存在某一边的叶子节点，则考虑父节点的存在，若父节点存在并且自身没有叶子节点则父节点标记为1，否则将自身标记为1，总数加1。  
看了题解，用三个状态的结构题深度优先搜索，只能说，这种解法是真的难想出来，强无敌的解法。  
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
private:
    bool leftFlag = true;
    bool rightFlag = false;
public:
    bool check(TreeNode* root) {
        return !root->left && !root->right;
    }

    int mark(TreeNode *root) {
        bool left = root->left && check(root->left); 
        bool right = root->right && check(root->right);
        if ((left && right) || (left && !root->right) || (right && !root->left)) {
            root->val = 1;
            return 1;
        } else return 0;
    }
    
    int dfs(TreeNode* root,TreeNode* parent) {
        int sum = mark(root);
        bool left = root->left && !check(root->left);
        bool right = root->right && !check(root->right);
        if (left && right) {
            sum += dfs(root->left,root);
            sum += dfs(root->right,root);
            if (root->val == 1) {
                sum += 1;
            } else {
                if (root->left->val != 1 && root->right->val != 1) {
                    if (parent) {
                        parent->val = 1;
                    } else {
                        root->val = 1;
                        sum += 1;
                    }
                }
            }
        } else if (left) {
            sum += dfs(root->left,root);
            if (root->val == 1) {
                sum += 1;
            } else {
                if (root->left->val != 1 || root->right) {
                    if (root->right || !parent) {
                        root->val = 1;
                        sum += 1;
                    } else {
                        parent->val = 1;
                    }
                } 
            }
        } else if (right) {
            sum += dfs(root->right,root);
            if (root->val == 1) {
                sum += 1;
            } else {
                if (root->right->val != 1 || root->left) {
                    if (root->left || !parent) {
                        root->val = 1;
                        sum += 1;
                    } else {
                        parent->val = 1;
                    }
                }
            }
        }
        return sum;
    }

    int minCameraCover(TreeNode* root) {
        if (!root->left && !root->right) return 1;
        return dfs(root,nullptr);
    }
};
```