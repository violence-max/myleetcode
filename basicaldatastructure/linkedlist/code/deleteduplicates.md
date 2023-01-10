题目描述：  
![image](/basicaldatastructure/linkedlist/image/image11.png)  
解决过程：  
这道题是怎么能写一个小时的。。。  
我感觉我对于假头结点又忘记怎么写了，一直在纠结头结点应该怎么处理，还在纠结pre节点的处理逻辑，我真的吐了。  
算法流程：  
1. dummyhead设置为一个指向head的新节点，cur指向head，pre指向dummyhead
2. 当cur不为空时持续while循环，循环体里面使用next接收cur的下一个节点，当next不为空而且next指向的val和cur指向的val相等时，往后移动next直到next指向的val的值和cur指向的val的值不想等为止，然后pre的下一个节点指向next。如果next指向的val的值与cur指向的val的值不相等，则让pre指向cur。每次while循环末尾将cur的值重新指向next
3. 返回dummyhead的下一个节点  
代码：（我的→题解）  
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
        ListNode* cur = head,*dummyhead = new ListNode(0,head);
        ListNode* pre = dummyhead;
        while (cur) {
            ListNode* next = cur->next;
            if (next && next->val == cur->val) {
                while (next && next->val == cur->val)
                    next = next->next;
                pre->next = next;
            } else {
                pre = cur;
            }
            cur = next;
        }
        return dummyhead->next;
    }
};
```  
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
        ListNode* dummyhead  = new ListNode(0,head);
        ListNode* cur = dummyhead;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int x = cur->next->val;
                while (cur->next && cur->next->val == x) 
                    cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }
        return dummyhead->next;
    }
};
```