题目描述：  
![image](/basicaldatastructure/linkedlist/image/image20.png)  
解决过程：  
五分钟ac，但是忽略了a是头节点的情况  
代码：  
```cpp
class Solution {
public:
    ListNode* movelist(ListNode* head, int index) {
        int i = 1;
        while (i++ != index) {
            head = head->next;
        }
        return head;
    }

    ListNode* mergeInBetween(ListNode* list1, int a, int b, ListNode* list2) {
        ListNode* node1 = movelist(list1,a), * node2 = movelist(list1,b);
        ListNode* anode = node1->next, * bnode = node2->next;
        node1->next = list2; 
        ListNode* tmp = list2;
        while (tmp->next) {
            tmp = tmp->next;
        }
        tmp->next = bnode->next;
        return list1;
    }
};
```