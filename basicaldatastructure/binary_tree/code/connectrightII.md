题目描述：  
![image](/basicaldatastructure/binary_tree/image/image12.png)  
解题过程：  
以为这个题跟第一个题一模一样，就是让我们写一遍空间复杂的为O(1)的的解法而已，但是后来才发现，原来第一个题是一颗完美二叉树。。。  
于是这个题就出现了一些小问题：  
1. 要考虑相邻节点之间左右子节点是否存在的连接问题  
2. 要考虑每次进行下一层寻找节点的时候的循环条件，并且需要找到一个具有孩子的节点进行判断  
3. 要考虑同层节点之间相邻节点之间孩子是否存在的的问题  
然后就花了一些时间去解决这三个问题。  
看了题解以后。。。原来还有dummy和pre的优化方法，代码直接少了好多行  
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
        Node *leftMost = root;
        while (leftMost && (leftMost->left || leftMost->right)) {
            Node *head = leftMost;
            while (head) {
                if (head->left && head->right) {
                    head->left->next = head->right;
                }
                Node *pre = head,*cur = head->next;
                while (cur && !cur->left && !cur->right)
                    cur = cur->next;
                if (cur) {
                    if (pre->right && cur->left) {
                        pre->right->next = cur->left;
                    } else if (pre->right && cur->right) {
                        pre->right->next = cur->right;
                    } else if (pre->left && cur->left) {
                        pre->left->next = cur->left;
                    } else if (pre->left && cur->right) {
                        pre->left->next = cur->right;
                    }
                }
                head = cur;
            }
            if (leftMost->left) leftMost = leftMost->left;
            else if (leftMost->right) leftMost = leftMost->right;
            while (leftMost && !leftMost->left && !leftMost->right)
                leftMost = leftMost->next;
        }
        return root;
    }
};
```