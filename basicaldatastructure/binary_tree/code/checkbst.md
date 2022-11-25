题目描述：  
![image](/basicaldatastructure/binary_tree/image/image31.png)  
解题思路：  
原来我对BST一直都有误解，我原来以为的BST就是左节点的值小于根节点，右节点的值大于根节点，然后就写了一个简易的递归版本对于每一个内节点进行判断，而且还写了十几分钟。。。一提交发现根本不能通过，因为右子树出现了一个比根节点要小的值。然后我想了一种方法，设置两个全局变量，一个是左，一个是右，左子树的所有节点的值必须都小于左，右子树的所有节点的值必须都大于右，于是我又一次验证，没有提交我就知道不行了，因为再进入不是根节点的内节点的时候需要重新更改左值和右值，这样在某些很偏的子树就需要更改这些值而导致与其靠近的其他子树无法正确得到判断。于是我认为应该让每个内节点都具有一个左值和右值，然后在自上而下递归的过程中不断改变，然而我想不出来好的处理这个思路的办法，直到，我看了题解，题解简直强无敌，用区间的形式把范围限制得恰到好处，而且在递归的过程中左值和右值还得到了很好的改变。所以，当涉及递归且需要重复使用某一个值的时候最好就是好好思考递归的思路。  
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
    bool check(TreeNode* root,long long lower,long long upper) {
        if (!root) return true;
        if (root->val <= lower || root->val >= upper) return false;
        return check(root->left,lower,root->val) && check(root->right,root->val,upper);
    }
    bool isValidBST(TreeNode* root) {
        return check(root,LONG_MIN,LONG_MAX);
    }
};
```