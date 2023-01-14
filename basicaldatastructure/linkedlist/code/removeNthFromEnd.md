题目描述：  
![image](/basicaldatastructure/linkedlist/image/image5.png)
解题过程：  
直接计算链表长度然后移动length-n次就找到要删除的那个结点了，然后记录了前结点，改变一下指针指向就完事了。看了题解发现这个方法的名字叫链表长度法，而且把获取链表长度的方法封装起来了。我做这个方法的时候还是想得很周到的，把一些可能失败的情况都给解决掉了。题解里面还有另外两个方法，栈方法啊，还有双指针法啊，都挺不错的。  
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *newHead = head;
        int length = 0;
        while (head) {
            length++;
            head = head->next;
        }
        if (length == 0) return nullptr;
        if (n == length) {
            ListNode *resultHead = newHead->next;
            delete newHead;
            return resultHead;
        }
        int i = 0;
        ListNode *pre = nullptr,*cur = newHead;
        while (i++ != length - n) {
            pre = cur;
            cur = cur->next;
        }
        pre->next = cur->next;
        delete cur;
        return newHead;
    }
};
```