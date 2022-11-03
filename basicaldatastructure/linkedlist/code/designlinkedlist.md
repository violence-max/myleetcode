题目描述：  
![image](/basicaldatastructure/linkedlist/image/image2.png)
解题过程：  
对这种需要自己设计数据结构的东西真的是很害怕，因为感觉自己写代码很少设计数据结构而且不经常用结构体，对一个数据结构里面该装有什么东西都不是很清楚。这道题要么就是单链表来解决，要么就是双向链表或者循环链表之类的链表，光是写个结构体都有点小困难了。后来知道了c++里有个现成的ListNode()类是单链表结点，但是我写的是双向链表，所以要自己写结构体，写结构体比较tricky的地方是结构体里面的初始化，就是类似于构造函数的写法，不是很清楚。写完结构体和在类里进行了初始化以后一切就都很顺利了，因为就是找索引或者改指针指向，都没什么难度，但是就是这么没有难度的东西做了一个多小时。  
写的时候有两个比较纠结的地方：  
1. 经常出现的找索引问题
2. 该不该用this
代码：  
```cpp
struct DLinkedList {
    int val;
    DLinkedList *pre,*next;
    DLinkedList(int _val) : val(_val),pre(nullptr),next(nullptr) {}
};

class MyLinkedList {
public:
    MyLinkedList() {
        this->size = 0;
        this->dummyHead = new DLinkedList(0);
        this->dummyTail = new DLinkedList(0);
        this->dummyHead->next = this->dummyTail;
        this->dummyTail->pre = this->dummyHead; 
    }
    
    int get(int index) {
        if (index < 0 || index >= this->size) return -1;
        DLinkedList *temp = find(index);
        return temp->val;
    }
    
    void addAtHead(int val) {
        this->size++;
        DLinkedList *newNode = new DLinkedList(val);
        newNode->next = this->dummyHead->next;
        newNode->pre = this->dummyHead;
        this->dummyHead->next->pre = newNode;
        this->dummyHead->next = newNode;
    }
    
    void addAtTail(int val) {
        this->size++;
        DLinkedList *newNode = new DLinkedList(val);
        newNode->next = this->dummyTail;
        newNode->pre = this->dummyTail->pre;
        this->dummyTail->pre->next = newNode;
        this->dummyTail->pre = newNode;
    }
    
    void addAtIndex(int index, int val) {
        if (index < 0) {
            addAtHead(val);
        } else if (index == this->size) {
            addAtTail(val);
        } else if (index > this->size) {
            return;
        } else {
            DLinkedList *temp = find(index);
            DLinkedList *newNode = new DLinkedList(val);
            newNode->next = temp;
            newNode->pre = temp->pre;
            temp->pre->next = newNode;
            temp->pre = newNode;
            this->size++;
        }
    }
    
    void deleteAtIndex(int index) {
        if (index < 0 || index >= this->size) return;
        DLinkedList *temp = find(index);
        temp->pre->next = temp->next;
        temp->next->pre = temp->pre;
        this->size--;
        delete temp;
    }

    DLinkedList *find(int index) {
        if (index >= this->size || index < 0) return nullptr;
        DLinkedList *temp;
        if (index + 1 < this->size - (index + 1)) {
            int i = 0;
            temp = this->dummyHead->next;
            while (i++ != index) {
                temp = temp->next;
            }
        } else {
            int i = this->size - 1;
            temp = this->dummyTail->pre;
            while (i-- != index) {
                temp = temp->pre;
            }
        }
        return temp;
    }
private:
    int size;
    DLinkedList *dummyHead;
    DLinkedList *dummyTail;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```