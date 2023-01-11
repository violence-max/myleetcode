题目描述：  
![image](/basicaldatastructure/linkedlist/image/image12.png)  
解题过程：  
因为我已经做过II了，所以这题两分钟就AC了，很简单的，代码说明一切。但是我看了题解之后发现不用dummyhead也可以，也许这一点可以纳入以后做链表题的考虑范围中来  
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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummyhead = new ListNode(0,head);
        ListNode* cur = dummyhead;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                cur = cur->next;
                int x = cur->val;
                while (cur->next && cur->next->val == x) {
                    cur->next = cur->next->next;
                }
            } else {
                cur = cur->next;
            }
        }
        return dummyhead->next;
    }
};
```