题目描述：  
![image](/basicaldatastructure/binary_tree/image/image35.png)  
解题过程：  
没想到这道题居然花了我四十多分钟的时间，看到题解众人说简单的时候我都麻了。  
一开始我做这道题想用抽象的思路去解决，因为是一颗二叉搜索树嘛，所以每次只需要判断当前节点值和给定要插入的节点值得大小关系就可以得出是要插入到哪个位置了，所以我抽象了一个插入函数，一开始是左右插入函数都写了一个，后面发现可以合并。我以为二叉树的指针在函数里面是可以直接改变的，讲道理确实可以直接改变，但是我可能方法有问题，一位内我的参数里面有根节点还有子节点，本来我指望着这几个节点可以在函数里面发生改变，结果不能，这里mark一下，以后找到答案以后回答。所以我被迫把我的抽象insert函数从void改成了能够返回一个节点指针类型的函数，然后返回根节点。但是，还是出现了错误，这个错误发生在向空的孩子进行插入时，我发现直接对child赋值不能让head出现同样的赋值，然后又加了判断语句，决定是向head的右孩子还是左孩子进行赋值。最后，没有想到我的二叉搜索树插入逻辑有问题，我本来以为如果给定当前值如果大于孩子，那么就可以插入在根节点和孩子中间，但是没有想到孩子的右孩子还可以比孩子大，所以一个给定的值应该插在最底层节点的一个空的左孩子或者右孩子处才对。  
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
    bool RIGHTFLAG = true;
    bool LEFTFLAG = false;
public:
    TreeNode* insert(TreeNode* head,int val,bool &flag,bool lorr) {
        TreeNode* child = (lorr) ? head->right : head->left;
        if (child) {
            head =child;
        } else {
                child = new TreeNode(val);
                if (lorr) head->right = child;
                else head->left = child;
                flag = true;
        }
        return head;
    }

    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) return new TreeNode(val);
        TreeNode* head = root;
        bool flag =false;
        while (!flag) {
            if (val > head->val) {
                head = insert(head,val,flag,RIGHTFLAG);
            } else {
                head = insert(head,val,flag,LEFTFLAG);
            }
        }
        return root;
    }
};
```