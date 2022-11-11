题目描述：  
![image](/basicaldatastructure/linkedlist/image/image7.png)  
解题过程：  
一开始采用了一个非常蠢得暴力遍历法：以为循环每个结点，然后对于后面的结点不断循环，如果回到开头就存在环，但是这个方法不清楚的地方是它不能提前直到环在哪开始，因此完全会陷入死循环。  
之后想到了哈希表辅助的方法，对于每个结点进行循环，判断结点是否在哈希表中，如果在则说明存在环，返回结点，如果不存在则继续循环。  
看了题解，还有另外一种方法：就是快慢指针法，具体见题解了，总之是一种需要数学和图形进行结合的方法，很巧妙。  
代码：  
1. 哈希表  
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
    ListNode *detectCycle(ListNode *head) {
        ListNode *tempHead = head,*tempNode;
        unordered_map<ListNode*,int> circleTest;
        while (tempHead) {
            if (circleTest.find(tempHead) != circleTest.end()) {
                return tempHead;
            } else {
                circleTest[tempHead]++;
                tempHead = tempHead->next;
            }
        }
        return nullptr;
    }
};
```
2. 双指针  
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
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head,*slow = head;
        while (fast) {
            slow = slow->next;
            if (!fast->next) return nullptr;
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *temp = head;
                while (temp != slow) {
                    temp = temp->next;
                    slow = slow->next;
                }
                return temp;
            }
        }
        return nullptr;
    }
};
```