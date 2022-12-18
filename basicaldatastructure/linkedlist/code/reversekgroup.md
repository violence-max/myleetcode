题目描述：  
![image](/basicaldatastructure/linkedlist/image/image10.png) 
解决过程：  
递归秒杀，什么乐色题目，就是指针的操作而已，最经典的还是反转链表的指针操作  
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
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode *tmp = head;
        int tempK = k;
        while (head) {
            head = head->next;
            tempK--;
            if (tempK == 0) break;
        }
        if (tempK) return tmp;
        else {
            ListNode* pre = nullptr,*cur = nullptr;
            ListNode *root = tmp;
            tempK = k;
            while (tmp) {
                pre = cur;
                cur = tmp;
                tmp = tmp->next;
                cur->next = pre;
                tempK--;
                if (tempK == 0) break;
            }
            root->next = reverseKGroup(tmp,k);
            return cur;
        } 
    }
};
```