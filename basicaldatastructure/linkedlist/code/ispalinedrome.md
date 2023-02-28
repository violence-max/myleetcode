题目描述：  
![image](/basicaldatastructure/linkedlist/image/image21.png)  
解决过程：  
状态好差哎  
代码（栈→递归→反转链表）  
```cpp
class Solution {
public:
    ListNode* middlenode(ListNode* head) {
        if (!head) return nullptr;
        ListNode* slow = head, * fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    int getlen(ListNode* head) {
        int result = 0;
        while (head) {
            result++;
            head = head->next;
        }
        return result;
    }

    bool isPalindrome(ListNode* head) {
        stack<ListNode*> stk;
        ListNode* minode = middlenode(head);
        ListNode* cur = head;
        while (cur != minode) {
            stk.push(cur);
            cur = cur->next;
        }
        int len = getlen(head);
        if ((len & 1) == 0) stk.push(cur);
        ListNode* another = minode->next;
        while (!stk.empty()) {
            if (stk.top()->val != another->val) {
                return false;
            }
            stk.pop();
            another = another->next;
        }
        return true;
    }
};
```
```cpp
class Solution {
    ListNode* frontPointer;
public:
    bool recursivelyCheck(ListNode* currentNode) {
        if (currentNode != nullptr) {
            if (!recursivelyCheck(currentNode->next)) {
                return false;
            }
            if (currentNode->val != frontPointer->val) {
                return false;
            }
            frontPointer = frontPointer->next;
        }
        return true;
    }

    bool isPalindrome(ListNode* head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
};
```  
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == nullptr) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode* firstHalfEnd = endOfFirstHalf(head);
        ListNode* secondHalfStart = reverseList(firstHalfEnd->next);

        // 判断是否回文
        ListNode* p1 = head;
        ListNode* p2 = secondHalfStart;
        bool result = true;
        while (result && p2 != nullptr) {
            if (p1->val != p2->val) {
                result = false;
            }
            p1 = p1->next;
            p2 = p2->next;
        }        

        // 还原链表并返回结果
        firstHalfEnd->next = reverseList(secondHalfStart);
        return result;
    }

    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    ListNode* endOfFirstHalf(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```