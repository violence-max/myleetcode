题目描述：  
![image](/basicaldatastructure/binary_tree/image/image11.png)    
解题过程：  
这到题还花了一点时间哦，主要是因为在想每一层之间应该怎样进行连接，一开始是想着从队头拿出一个节点，如果此时队列未空则进行节点之间的连接，就是一个广度遍历嘛，但是，因为我的判断条件是队列是否为空，而且在同一个队列里还会一直不断地push新节点，所以很可能会进行不同层次节点之间的连接，为了解决这个问题，我采取的方案是用一个额外的队列，一个队列进行同层之间节点的构建，一个队列进行下一层节点的压入，同层节点之间的构建结束之后再把下一层节点压入第一个队列中。但是有一种比较简便的办法是可以直接判断当前队列队头节点是否是同层节点的，直接判断count的值就完了。。。（题解的方法）我感觉我每次想到的解决方案都好渣渣啊。  
题解还有一种空间复杂度为1的解法，是真的寂寞，直接拿到队头元素然后一层一层进行构建  
代码：  
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return root;
        queue<Node*> q1,q2;
        q1.push(root);
        while (!q1.empty()) {
            int count = 0,size = q1.size();
            while (count++ < size) {
                Node *node = q1.front(); q1.pop();
                if (!q1.empty()) {
                    Node *next = q1.front(); 
                    node->next = next;
                } else {
                    node->next = nullptr;
                }
                if (node->left) q2.push(node->left);
                if (node->right) q2.push(node->right);
            }
            count = 0; size = q2.size();
            while (count++ < size) {
                Node *node = q2.front(); q2.pop();
                q1.push(node);
            }
        }
        return root;
    }
};
```