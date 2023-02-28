题目描述：  
![image](/basicaldatastructure/linkedlist/image/image14.png)  
我用的方法是直接在原链表上修改节点之间的指针的关系，这道题居然写了半个小时我去，太菜了我。  
这道题也算是经典算法吧，记录一下我的算法流程：  
1. 设置四个指针，分别是l，记录位置为left的节点，初值为nullptr，before，记录l之前的一个节点，初值为nullptr，cur，记录当前节点，初值为head，pre，记录cur之前的一个节点  
2. 使用pos ≤ right作为判断条件来进行while循环，pos初值为0：
    1. 每次进行循环时先将pos的值加1，pos表示当前遍历到的第pos个节点，从1开始计数
    2. 用next指针记录cur的下一个节点，因为如果发生节点之间的指针的指向关系的变化，cur的下一个节点会丢失
    3. 如果pos等于left，则给l赋值cur，说明当前遍历到第left个节点；如果pos大于left，则说明要进行节点之间指针指向的改变：
        首先，pre一定不为空，因为当pos大于left时cur一定经历了left节点。因此，可以另pre的下一个节点为cur的下一个节点，cur的下一个节点为l。如果before不为空，则说明l之前有节点，则另before的下一个节点为cur。这是经典的头插法策略。但若before为空，则无需进行此操作，此时cur应该作为新的节点，需要另head等于cur。最后将l更新为cur，符合头插法规范即可  
    4. 不论是节点之间的指向发生改变还是没有发生改变，before、pre以及cur都应该发生相应的改变以便遍历链表并做出正确的操作：
        如果l已经找到了，即不为空，则before无需发生改变，一直在l节点之前即可，否则需要赋值为cur来寻找l节点之前的节点  
        如果cur的下一个节点和next节点不相等，则说明发生了节点之间指向的改变，此时不应该改变pre，因为pre恰好在next之前，否则需要赋值为cur来更新cur之前的节点  
        cur每次赋值为next就好  
    5. 如果pos等于right说明已经完成了最后一个节点的改变，退出循环即可
3. 最终返回头结点head 
递归没那么多时间讲了，还有直接切割链表然后反转链表再拼接的方法  
代码：（头插法→递归→反转链表）  
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
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if (left == right) return head;
        ListNode* l = nullptr,* pre = nullptr,* cur = head,* before = nullptr;
        int pos = 0;
        while (pos <= right) {
            ListNode* next = cur->next;
            pos += 1;
            if (pos == left) l = cur;
            else if (pos > left) {
                pre->next = cur->next;
                cur->next = l;
                if (!before) {
                    head = cur;
                } else before->next = cur;
                l = cur;
            }
            pre = (next == cur->next) ? cur : pre;
            before = (l) ? before : cur;
            cur = next;
            if (pos == right) break;
        }
        return head;
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
private:
    int i = 0;
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        i++;
        if (i == right) {
            return head;
        }
        if (i < left) {
            head->next = reverseBetween(head->next,left,right);
            return head;
        } else {
            ListNode* node = reverseBetween(head->next,left,right);
            ListNode* next = head->next->next;
            head->next->next = head;
            head->next = next;
            return node;
        }
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
    ListNode* reverselist(ListNode* head) {
        ListNode* pre = nullptr,* cur = head;
        while (cur) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }

    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* dummyHead = new ListNode(-1,head);
        ListNode* pre = dummyHead;
        for (int i = 0; i < left - 1; i++) {
            pre = pre->next;
        }
        ListNode* leftnode = pre->next;
        ListNode* rightnode = pre;
        for (int i = 0; i < right - left + 1; i++) {
            rightnode = rightnode->next;
        }
        ListNode* next = rightnode->next;
        rightnode->next = nullptr;
        pre->next = nullptr;
        pre->next = reverselist(leftnode);
        leftnode->next = next;
        return dummyHead->next;
    }
};
```