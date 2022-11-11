题目描述：  
![image](/basicaldatastructure/linkedlist/image/image3.png)
解题过程：  
一开始想到的是迭代的方法，但是一开始写的是使用额外的空间，就是每扫描一个结点就新建一个值一样的结点来结合头插法加入到链表中，后面优化了这个迭代的方法，直接扫描一遍把链表倒转过来。看了题解学了递归的方法。  
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
    ListNode* reverseList(ListNode* head) {
        if (!head) return nullptr;
        ListNode *resultHead = nullptr,*temp;
        while (head) {
            temp = head;
            head = head->next;
            temp->next = resultHead;
            resultHead = temp;
        }
        return resultHead;
    }
};
```
2. 递归
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
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode *newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```