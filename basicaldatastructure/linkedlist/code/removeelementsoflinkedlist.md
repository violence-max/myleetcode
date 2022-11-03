题目描述：  
![image](/basicaldatastructure/linkedlist/image/image1.png)
解题过程：  
直接写就好了，我写的是先找到第一个不是val值得结点，然后对于之后的结点遇到值相等的就改变指针的指向。  
题解里面的递归的方法挺不错的，遇到是val值的结点就直接返回下一个结点。迭代的方法用到了一个O(1)的额外空间，从头结点之前的一个结点开始，也很nice  
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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *pre = head;
        while (pre && pre->val == val) {
            pre = pre->next;
        }
        if (!pre) return nullptr;
        ListNode *resultHead = pre,*cur = pre->next;
        while (cur) {
            if (cur->val == val) {
                pre->next = cur->next;
            } else {
                pre = cur;
            }
            cur = cur->next;
        }
        return resultHead;
    }
```