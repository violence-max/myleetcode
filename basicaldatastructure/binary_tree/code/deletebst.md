题目描述：  
![image](/basicaldatastructure/binary_tree/image/image36.png)    
解题过程：  
先是思考了很久的二叉树的删除操作应该怎么完成，经过自己的思考，我得出的答案是在分类讨论的情况下只有带删除节点均存在左右孩子时值得注意，我想出来的是把左孩子作为右孩子最左节点的左孩子。但是看了题解发现，这种做法会让树的高度变高，不便于搜索，还有一种方法是把右孩子作为左孩子的最右孩子的右节点。最好的方法应该是将右孩子的最左节点（称为待删除节点的**后继节点**）作为新节点，其右孩子是删除了该节点过后的右孩子，左孩子是待删除节点的左孩子。这种做法不会增加树的高度，是最棒的。  
题解里面有递归的解法，写得非常好，很清晰。我还看到了评论里面有一种二级指针的写法，这种写法也是真的寂寞，强行用二级指针把左子树合并到右子树，然后返回右子树。二级指针的出现也解答了为什么我在做二叉树的插入的时候，不能形成在插入函数里的修改，原来就是没有对一个节点的地址做操作，实际上还是一个传值调用。  
我自己的做法使用了leftf指代左父节点，rightf指代了右父节点，初始时两个指针设置为空，没向左或者向右移动head时对其中一个进行赋值，另外一个进行“复位”，找到待删除结点后根据这两个指针来知道应该应该从哪个节点的哪个孩子进行拼接。  
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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root || (!root->left && !root->right && root->val == key)) return nullptr;
        TreeNode *head = root,*rightf = nullptr,*leftf = nullptr;
        while (head) {
            if (head->val == key) {
                if (!head->left && !head->right) {
                    if (leftf) leftf->left = nullptr;
                    if (rightf) rightf->right = nullptr;
                } else if (head->left && head->right) {
                    if (leftf) leftf->left = head->right;
                    if (rightf) rightf->right = head->right;
                    TreeNode* node = head->right;
                    while(node) {
                        if (node->left) node = node->left;
                        else break;
                    }
                    node->left = head->left;
                    if (!leftf && !rightf) return head->right;
                } else if (head->left) {
                    if (!leftf && !rightf) return head->left;
                    if (leftf) leftf->left = head->left;
                    if (rightf) rightf->right = head->left;
                } else {
                    if (!leftf && !rightf) return head->right;
                    if (leftf) leftf->left = head->right;
                    if (rightf) rightf->right = head->right;
                }
                break;
            } else if (head->val > key) {
                leftf = head; rightf = nullptr;
                head = head->left;
            } else {
                rightf = head; leftf = nullptr;
                head = head->right;
            }
        }
        return root;
    }
};
```  
二级指针：  
最后直接把待删除节点的地址给修改了，以及直接对后继节点进行操作，极其离谱  
```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        TreeNode **p = &root; //指向待删除节点的指针
        while(*p && (*p)->val != key) 
            p = (*p)->val < key ? &(*p)->right : &(*p)->left;
        if(!*p) return root;
        TreeNode **t = &(*p)->right; //指向右树的最左空指针的指针（这里把左树合到右树，反之同理即可）
        while(*t) t = &(*t)->left;
        *t = (*p)->left;
        *p = (*p)->right;
        return root;
    }
};
```