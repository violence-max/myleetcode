题目描述：  
![image](/basicaldatastructure/binary_tree/image/image52.png)  
解决过程：  
本来想着对每个节点都进行一次计算就可以了，但是居然把这一步放到了实际计算的helper函数中。题解中的思路最终点的部分就在于在pathSum和helper两个函数里都进行了深度优先搜索  
- 前缀和：  
在深度优先搜索的过程中记录根节点到每个节点的路径之和，当遍历到节点node时，得到根节点root到node的路径和prefix，如果满足在pre哈希map中存在值prefix - targetsum，则说明可以在这条路径上去掉该条路径的前部分节点来实现从上到下且权值之和为pathsum的路径  
要点：  
1. 在对左子树和右子树进行递归之前先对pre哈希map进行了值加入的操作，在结束了对左子树和右子树的遍历后在哈希map中减去了对应的值，防止同一个根节点的左子树的前缀和影响到右子树  
代码：（深度优先搜索→前缀和）  
```cpp
class Solution {
public:
    int helper(TreeNode* root, long long _targetSum) {
        if (!root) return 0;
        // cout << root->val << ' ' << _targetSum << endl;
        int res = 0;
        if (root->val == _targetSum) {
            res++;
        }
        res += helper(root->left,_targetSum - root->val) + helper(root->right, _targetSum - root->val);
        return res;
    }

    int pathSum(TreeNode* root, int targetSum) {
        if (!root) return 0;
        int res = 0;
        res += helper(root,targetSum);
        res += pathSum(root->left,targetSum) + pathSum(root->right, targetSum);
        return res;
    }
};
```  
```cpp
class Solution {
public:
    unordered_map<long long, int> prefix;

    int dfs(TreeNode *root, long long curr, int targetSum) {
        if (!root) {
            return 0;
        }

        int ret = 0;
        curr += root->val;
        if (prefix.count(curr - targetSum)) {
            ret = prefix[curr - targetSum];
        }

        prefix[curr]++;
        ret += dfs(root->left, curr, targetSum);
        ret += dfs(root->right, curr, targetSum);
        prefix[curr]--;

        return ret;
    }

    int pathSum(TreeNode* root, int targetSum) {
        prefix[0] = 1;
        return dfs(root, 0, targetSum);
    }
};
```