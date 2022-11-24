题目描述：  
![image](/basicaldatastructure/binary_tree/image/image26.png)  
解题过程：  
这个题根本不会好吧，直接看题解。  
题解真的太寂寞了。从中序遍历和后序遍历来构建二叉树最tricky的地方在于中序遍历和后序遍历的特点。中序遍历是先左后根后右，后序遍历是先左后右再根，中序遍历反过来就是先右后根，而后序遍历反过来就是先根后右，这两段序列是一样的！！！这样就可以用递归或者栈的思想做出来了，所以递归和栈两种解法都是可行的。  
递归：  
先通过中序遍历数组构建一个哈希map存储中序遍历中键到索引的映射，然后从后序遍历数组的最后一个值开始递归构建。对于每一个值都创建一个节点，然后对于当前值，在哈希map中获得索引位置，认为当前值是树的一个子结构的根节点，然后从中序遍历数组当前索引位置的右边递归构建右子树，左边递归构建左子树。
但是我觉得递归这个方法不能处理当一颗二叉树有很多相同的值的情况。  
（突然意识到迭代好像也不能处理有很多相同值的情况，会出现栈不断弹出哈哈哈，那算了）  
迭代和递归都是一样的，从后序遍历数组除了最后一个节点以外的节点从后往前开始进行构建，先让根节点入栈，设置一个指针从中序遍历数组最后一个节点值开始扫描，如果栈顶的值与指针的值不相等，则取后序遍历数组当前值的节点作为栈顶节点的右孩子，否则在栈非空的情况下不断弹出与指针值相等的栈顶元素，每次弹出指针都要进行一次左移 
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
    int postidx;
    unordered_map<int,int> inorderidx;
public:
    TreeNode* recur(int left,int right,vector<int> inorder,vector<int> postorder) {
        if (left > right) return nullptr;
        TreeNode *root = new TreeNode(postorder[postidx--]);
        int idx = inorderidx[root->val];
        root->right = recur(idx + 1,right,inorder,postorder);
        root->left = recur(left,idx - 1,inorder,postorder);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (!(int)postorder.size()) return nullptr;
        postidx = (int)postorder.size() - 1;
        int idx = 0;
        for (auto val : inorder) {
            inorderidx[val] = idx++;
        }
        return recur(0,(int)inorder.size()-1,inorder,postorder);
    }
};
```