题目描述： 
![image](/basicaldatastructure/linkedlist/image/image15.png) 
解决过程： 
我一开始想的是直接进行k次旋转，第一次提交发现忽略了头节点为空的情况，第二次提交发现超时了。确实，因为这是一个循环，直接进行一个k对n的取模，然后旋转这么多次就好了。  
我敲，取模的时候会出现模为0的情况嘛，就是k是链表长度的倍数，我一开始的处理方法是如果k等于0就赋值成链表的长度n，然后进行k次的旋转，看了题解我发现这种处理方法简直蠢到家了，在取模之后直接添加一条判断语句如果k为0直接返回头节点就好了。后来又发现这样子做居然执行用时多了4ms，仔细一想，如果k是0其实接下来的旋转循环根本不会发生旋转，也是直接返回头节点，于是把判断语句给删掉了，然后执行用时少了8ms。看了判断语句挺费时间的。  
题解里面写得很精髓，没有用到多余的函数，寻找尾节点和求链表长度都在一个循环里面进行。而且题解的思路跟我不同：先把链表连成环，寻找到新的链表的尾节点，尾节点的下一个节点即为新的头节点，记录之，然后砍掉尾节点，返回新的头节点，我觉得还蛮行的。  
代码：  
我的：  
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
    int getLength(ListNode* head) {
        int len = 0;
        while (head) {
            len++;
            head = head->next;
        }
        return len;
    }

    ListNode* rotate(ListNode* head) {
        if (!head->next) return head;
        ListNode *tmp = head,*cur = head;
        while (cur) {
            if (cur->next && !cur->next->next) {
                break;
            }
            cur = cur->next;
        }
        ListNode* tail = cur->next;
        tail->next = head;
        cur->next = nullptr;
        return tail;
    }

    ListNode* rotateRight(ListNode* head, int k) {
        int len = getLength(head);
        if (!len) return nullptr;
        k = k % len;
        while (k--)
            head = rotate(head);
        return head;
    }
};
```  
官解 ：  
```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (k == 0 || head == nullptr || head->next == nullptr) {
            return head;
        }
        int n = 1;
        ListNode* iter = head;
        while (iter->next != nullptr) {
            iter = iter->next;
            n++;
        }
        int add = n - k % n;
        if (add == n) {
            return head;
        }
        iter->next = head;
        while (add--) {
            iter = iter->next;
        }
        ListNode* ret = iter->next;
        iter->next = nullptr;
        return ret;
    }
};
```