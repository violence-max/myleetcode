题目描述：  
![Untitled](/basicaldatastructure/binary_tree/image/image28.png)  
解题过程：  
我递归秒解，但是看了答案，嘻嘻。  
单调栈真的是一种寂寞的解法，用一个左数组和一个右数组维护每个索引位置上的值的左边和右边第一个最大的值，初始时所有值均为-1。  
将左数组和右数组求出来以后，从头开始遍历节点值，将既没有左大值和右大值的节点设置为头节点，如果有左大值而无右大值，则将左大值的右孩子设置为当前节点，如果有右大值而无左大值则将右大值的左孩子设置为当前节点，如果左右大值均有，则比较左右大值的大小，若左大值更大，则将右大值的左孩子设置为当前节点，若右大值更大，则将左大值的右孩子设置为当前节点  
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
    int findTheMax(int left,int right,vector<int> nums) {
        int max = INT_MIN,idx = INT_MIN;
        for (int i = left; i <= right; i++) {
            if (nums[i] > max) {
                max = nums[i];
                idx = i;
            }
        }
        return idx;
    }
    TreeNode *recur(int left,int right,vector<int> nums) {
        if (left > right) return nullptr;
        int idx = findTheMax(left,right,nums);
        TreeNode *root = new TreeNode(nums[idx]);
        root->left = recur(left,idx - 1,nums);
        root->right = recur(idx + 1,right,nums);
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if (!(int)nums.size()) return nullptr;
        return recur(0,(int)nums.size()-1,nums);
    }
};
```