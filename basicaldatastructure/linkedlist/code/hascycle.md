题目描述：  
![image](/basicaldatastructure/linkedlist/image/image17.png)  
解题过程：  
快慢指针瞬间ac，我还以为我做过这个题了，没想到做的是II，需要返回那个重复的节点  
代码：  
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head, * fast = head;
        while (fast) {
            slow = slow->next;
            fast = (fast->next) ? fast->next->next : fast->next;
            if (slow && slow == fast) return true;
        }
        return false;
    }
};
```