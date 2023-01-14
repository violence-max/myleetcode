题目描述：  
![image](/basicaldatastructure/linkedlist/image/image6.png)  
解题过程：  
直接暴力法了，对于链表A的每一个结点循环一次链表B，先判断两个结点是否相等（即有可能两个链表为互相包含的关系），然后判断两个结点的下一个结点是否相等，若都不是则移动链表B。我认为tricky的部分在于优化时间复杂度吧，暂时还没想出来。  
看了题解，感觉哈希表的insert和count操作很tricky，还有双指针的操作更tricky了，哈哈，就算一开始错过了，终究还是会相遇，太牛逼了。先不写代码，等到二刷或者什么时候有缘了再写。  
代码：  
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (!headA || !headB) return nullptr;

        ListNode *tempHeadB = headB;
        while (headA) {
            tempHeadB = headB;
            while (tempHeadB) {
                if (headA == tempHeadB) {
                    return headA;
                } else {
                    tempHeadB = tempHeadB->next;
                }
            }
            headA = headA->next;
        }
        return nullptr;
    }
};
```