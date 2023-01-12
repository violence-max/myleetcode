题目描述：  
![image](/basicaldatastructure/linkedlist/image/image13.png)  
解题过程：  
一开始想的方向居然是错误的，直到我碰到测试用例head = [3,1]，x = 2才发现。因为我一开始想的是链表中一定会存在一个值为x的节点，然后当第一次遍历到值为x的节点之后再重新遍历一次链表，把素有值小于x的节点进行移动，返回头节点就可以了，没有想到x居然只是一个参考量……如果是这样那就可以直接对遇到的值小于x的节点进行处理了，具体算法描述如下：  
1. 设置cur指针指向头节点head，方便后续遍历链表
2. 设置pre指针指向cur指针指向的节点的前一个节点，起初赋值为nullptr
3. 设置first指针记录第一个值大于等于x的节点，所有值小于x的节点都要按照相对位置移动到first指针指向的节点之前
4. 设置before指针记录first指针之前的一个节点，为了移动值小于x的节点
5. while循环，当cur不为空时持续循环：
    1. 如果当前节点值小于x而且first不为空，则说明需要进行节点移动：  
        pre指针的下一个节点指向cur指针的下一个节点（在这里pre指针不可能为空），cur指针的下一个节点指向first节点，如果before不存在说明当前的节点需要作为新的头结点，则令head = cur。cur指针移动结束之后before指针指向cur指针指向的节点进行更新，cur指针指向pre指针指向的下一个节点以进行下一次循环  
    2. 如果当前节点值大于等于x且first为空，则另first = cur对first进行赋值
    3. 如果当前节点值小于x且first为空，则说明还没遇到第一个值大于等于x的节点，则更新before，令before = cur
    4. 每次循环都更新pre节点和cur节点
6. 最后返回头节点head即可  
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
    ListNode* partition(ListNode* head, int x) {
        ListNode* cur = head,* pre = nullptr,* first = nullptr,* before = nullptr;
        while (cur) {
            if (cur->val < x && first) {
                pre->next = cur->next;
                cur->next = first;
                if (!before) {
                    head = cur;
                } else {
                    before->next = cur;
                }
                before = cur;
                cur = pre->next;
                continue;
            } else if (cur->val >= x && !first) {
                first = cur;
            } else if (cur->val < x && !first) {
                before = cur;
            }
            pre = cur;
            cur = cur->next;
        }
        return head;
    }
};
```