题目描述：  
![image](/basicaldatastructure/linkedlist/image/image8.png)  
解决过程： 
哎，最近心情是真的低落，非常影响我写代码的状态啊，不过也没有办法，生活就是这样。面对这种超简单的题目都写了十分钟，还出错了几次，有new的错误，有链表最后没有合并完的部分还一个一个合并的愚蠢，哎，还超时，忘记移动节点了  
代码：  
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *root1 = list1,*root2 = list2;
        ListNode *root = new ListNode();
        ListNode *dummy = root;
        while (root1 && root2) {
            if (root1->val < root2->val) {
                root->next = root1;
                root1 = root1->next;
            } else {
                root->next = root2;
                root2 = root2->next;
            }
            root = root->next;
        }
        while (root1) {
            root->next = root1;
            root = root->next;
            root1 = root1->next;
        }
        while (root2) {
            root->next = root2;
            root = root->next;
            root2 = root2->next;
        }
        return dummy->next;
    }
};
```