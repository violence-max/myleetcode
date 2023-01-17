题目描述：  
![image](/basicaldatastructure/binary_tree/image/image41.png)  
解决过程：  
这道题目，一开始我看错了，我以为要交换两个节点，没想到题目说的是在不考虑改变树的结构的条件下恢复二叉搜索树。想用中序遍历把出错的两个节点找出来，但是在分析了题目的例子过后我感觉我不能通过中序遍历的数组找出来哪两个节点出错了，因为我纠结单靠中序遍历无法确定一棵树，然而再把后序遍历拿出来就有点麻烦了。看了题解我发现我忽略了“交换”这两个字。出错的中序遍历数组一定是有两个节点发生交换过的。依据这一点，我终于撸出了代码，然而第一遍写的还是错的。发生出错的两个节点，第一个应该是第一段升序遍历的最大值，第二个应该是第二段升序遍历的最小值。  
显式中序遍历：用数组存储root的中序遍历结果再进行对哪两个值发生了错误的交换的判断（题解的写法是我想要的，我写的就是一坨，我太喜欢把各种情况统一起来的写法了。题解的思路是在错误的中序遍历当中至少会出现一次升序遍历的错误，此时用一个值记录升序遍历最后的一个有效值，另外一个值记录第一个破坏值，继续往下遍历重复这个过程，第二次直接退出即可。这里说得不详细，具体见隐式中序遍历的代码部分）  
隐式中序遍历：直接在中序遍历的过程中记录出错节点，不用数组进行存储。这里还复习了以下栈式的中序遍历写法，很nice。  
莫里斯遍历：没有想到我在学习红黑树的过程中已经把莫里斯遍历给学了，太爽了。笑死，红黑树的中序遍历方法根本就是作弊，因为有父节点。大名鼎鼎的莫里斯遍历果然牛逼，还是稍微看了一会才学明白。要命的是莫里斯遍历居然不允许提前退出，不然树中成环直接开席了，发现这一点花了我好久时间。这里就不记录莫里斯遍历的方法了吧，这道题花的时间已经够久了。move on！  
代码：（显式中序遍历→隐式中序遍历→莫里斯遍历）  
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
    vector<int> val; 
public:
    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        val.push_back(root->val);
        dfs(root->right);
    }

    void recoverTree(TreeNode*& root,int first,int second) {
        if (!root) return;
        if (root->val == first) {
            root->val = second;
        } else if (root->val == second) {
            root->val = first;
        }
        recoverTree(root->left,first,second);
        recoverTree(root->right,first,second);
    }

    void recoverTree(TreeNode* root) {
        dfs(root);
        int first = 0;
        for (int i = 1; i < val.size(); i++) {
            if (val[i] > val[i-1]) {
                first = i;
            } else break;
        }
        int second = first + 1;
        for (int i = second; i < val.size(); i++) {
            if (i + 1 < val.size() && val[i] > val[i+1]) {
                second = i + 1;
            }
        }
        first = val[first],second = val[second];
        recoverTree(root,first,second);
    }
};
```  
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
    void recoverTree(TreeNode* root) {
        TreeNode* x = nullptr;
        TreeNode* y = nullptr;
        TreeNode* pre = nullptr;
        stack<TreeNode*> stk;
        while (!stk.empty() || root) {
            while (root) {
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            if (pre && root->val < pre->val) {
                y = root;
                if (!x) x = pre;
                else break;
            }
            pre = root;
            root = root->right;
        }
        swap(x->val,y->val);
    }
};
```  
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
    void recoverTree(TreeNode* root) {
        TreeNode* x = nullptr ,*y = nullptr, *pred = nullptr, *predccessor = nullptr;
        while (root) {
            if (root->left) {
                predccessor = root->left;
                while (predccessor->right && predccessor->right != root) {
                    predccessor = predccessor->right;
                }
                if (!predccessor->right) {
                    predccessor->right = root;
                    root = root->left;
                } else {
                    if (pred && root->val < pred->val) {
                        y = root;
                        if (!x) x = pred;
                    }
                    pred = root;
                    predccessor->right = nullptr;
                    root = root->right;
                }
            } else {
                if (pred && root->val < pred->val) {
                    y = root;
                    if (!x) x = pred;
                }
                pred = root;
                root = root->right;
            }
        }
        swap(x->val,y->val);
    }
};
```