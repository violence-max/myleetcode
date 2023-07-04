题目描述：

![image](/basicaldatastructure/linkedlist/image/image24.png)

解题过程：

2分钟

代码：

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        function<ListNode*(ListNode*)> rev = [&](ListNode* head) -> ListNode* {
            ListNode*  cur = head, *pre = nullptr;
            while (cur) {
                ListNode* nxt = cur->next;
                cur->next = pre;
                pre = cur;
                cur = nxt;
            }
            return pre;
        };
        ListNode* rl1 = rev(l1), *rl2 = rev(l2);
        int c = 0;
        ListNode* dummyhead = new ListNode(0);
        ListNode* curr = dummyhead;
        while (rl1 || rl2 || c) {
            int x = rl1 ? rl1->val : 0;
            int y = rl2 ? rl2->val : 0;
            int num = x + y + c;
            c = num / 10;
            int val = num % 10;
            ListNode* next = new ListNode(val);
            curr->next = next;
            curr = next;
            rl1 = rl1 ? rl1->next : nullptr;
            rl2 = rl2 ? rl2->next : nullptr;
        }
        return rev(dummyhead->next);
    }
};
```