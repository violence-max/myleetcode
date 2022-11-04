题目描述：  
![image](/basicaldatastructure/linkedlist/code/swappairs.md)  
解题过程：  
一开始直接用迭代的思想去写这个题了，用了三个指针，一个pre，一个cur，一个tail，三个结点不断更改指针指向直到尾结点为空就退出循环就好了。看了题解看到了一种递归的方法，强无敌。  
代码：  
1. 迭代：  
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
    ListNode* swapPairs(ListNode* head) {
        if (!head) return nullptr;
        ListNode *pre = nullptr,*cur = head,*tail = head->next;
        ListNode *resulthead = (tail) ? tail : head;
        while (tail) {
            cur->next = tail->next;
            tail->next = cur;
            if (pre) pre->next = tail;
            pre = cur;
            cur = cur->next;
            if (!cur) break;
            else tail = cur->next;
        }
        return resulthead;
    }
};
```  
1. 递归  
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
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode *newHead = head->next;
        head->next = swapPairs(newHead->next);
        newHead->next = head;
        return newHead;
    }
};
```