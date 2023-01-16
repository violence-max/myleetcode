题目描述；  
![image](/basicaldatastructure/binary_tree/image/image40.png)  
这道题做了4个多小时才舍得看题解，看完题解，哎，我是傻逼。  
我一直以为我可以做到，选取一个数字为节点，然后对左边的部分进行深度优先搜索，右边的部分进行深度优先搜索。也就是说，整整四个多小时的时间，我都在思考怎么进行深度优先搜索。然而，就在某一时刻，我突然发现，当n稍微有点大时，选取i为根节点，不管是先对i的左子树部分进行深度优先搜索还是对i的右子树部分进行深度优先搜索，这都是不可取的—这是一个多对多映射！！也就是说，我必须要固定左边，然后对右边的每种情况做一个枚举， 才能得到其中一稞合适的树。然而，深度优先搜索根本做不到提前调回来然后又跳回去，懂我意思吧，不能先固定一边然后对另一边的所有情况进行进行枚举。这种方法只对n比较小的时候，左边的部分和右边的部分节点个数都不大于等于2的情况有效，这就是为什么前面我可以花这么多时间测试n=3的情况。一开始写的时候是真的要命，我用的是一个全局的root，然后不停地在更改root，我还把它加进答案数组里，也就是说，答案里只有一种情况的root，因为前面的会被后面的更改。而且各种方面都做得很烂，比如函数抽象的定义等等，完全就没搞清楚整个流程，就在那疯狂改。  
题解写的太好了，属于那种一看代码就明白的东西。所以我决定不记录流程。（我在搬题解的时候仍然犯了对root进行重写的错误，我认为我的状态出现了问题，然后去睡觉了）  
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
    vector<TreeNode*> generateTrees(int start,int end) {
        if (start > end) {
            return {nullptr};
        } else if (start == end) return {new TreeNode(start)};
        vector<TreeNode*> ans;
        ans.reserve(end - start + 1);
        for (int i = start; i <= end; i++) {
            vector<TreeNode*> left = generateTrees(start,i-1);
            vector<TreeNode*> right = generateTrees(i+1,end);
            for (auto& l : left) {
                for (auto& r : right) {
                    TreeNode* root = new TreeNode(i);
                    root->left = l;
                    root->right = r;
                    ans.push_back(root);
                }
            }
        }
        return ans;
    }

    vector<TreeNode*> generateTrees(int n) {
        return generateTrees(1,n);
    }
};
```