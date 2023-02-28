题目描述：  
![image](/basicaldatastructure/binary_tree/image/image45.png)
解决过程：  
靠，我以为的路径还以为必须是父节点-子节点-父节点-子节点这样，没有想到它说的是把二叉树当作是一个图，一笔画地连出一堆父节点和子节点就可以  
题解的代码相当简洁，语意也相当明了，因此细节见代码吧，感觉就是根节点到叶子节点路径最大和的翻版，只不过这个题目描述太sb了  
代码：  
```cpp
class Solution {
private:
    int maxsum = INT_MIN;
public:
    int maxGain(TreeNode* root) {
        if (!root) return 0;
        int leftGain = max(maxGain(root->left),0);
        int rightGain = max(maxGain(root->right),0);
        int newweight = root->val + leftGain + rightGain;
        maxsum = max(maxsum,newweight);
        return root->val + max(leftGain,rightGain);
    }

    int maxPathSum(TreeNode* root) {
        maxGain(root);
        return maxsum;
    }
};
```