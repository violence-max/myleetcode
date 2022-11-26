题目描述：  
![image](/basicaldatastructure/binary_tree/image/image38.png)  
解题过程：  
一开始以为可以取巧，写一条链式的二叉树，就是取中间值作为根节点，但是发现是要求高度平衡的。而且写这个方法的时候循环条件写得有问题，导致内存溢出了。。还看了挺久的感觉。后来意识到高度平衡，所以就改了，不知道是任督二脉打通了还是什么，就想到递归取中间节点进行二叉树构建，但是又发生了内存溢出的问题，原因是位运算的符号的优先级居然比加号低，无敌。  
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
    TreeNode* recur(vector<int> nums,int left,int right) {
        if (left > right) return nullptr;
        int mid = ((right - left) >> 1) + left;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = recur(nums,left,mid-1);
        root->right = recur(nums,mid+1,right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return recur(nums,0,(int)nums.size()-1);
    }
};
```