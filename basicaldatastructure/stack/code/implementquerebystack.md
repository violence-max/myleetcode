题目描述：  
![image](/basicaldatastructure/stack/image/image3.png)  
解题过程：  
这个题用两个数组实现就可以了，很经典的栈实现队列，但是用c++就可以使用一些api函数，用纯c那就sb了。我做这个题最大的考虑是如果pop的时候队列里面没有元素怎么办，但是定睛一看，居然提前说了空的队列不会调用pop操作，那就行。  
我不回stack啊，只能用vector了，vector居然没有pop_front函数，查了一下，有的是string。然后查了有关vector的api，用insert、at和erase来解决了。  
代码：  
```cpp
class MyQueue {
public:
    MyQueue() {

    }
    
    void push(int x) {
        stack1.insert(stack1.begin(),x);
    }
    
    int pop() {
        int result = peek();
        stack2.erase(stack2.begin());
        return result;
    }
    
    int peek() {
        if ((int)stack2.size() == 0) {
            while (stack1.size()) {
                stack2.insert(stack2.begin(),stack1.at(0));
                stack1.erase(stack1.begin());
            }
        }
        return stack2.at(0);
    }
    
    bool empty() {
        if (stack1.size() == 0 && stack2.size() == 0) return true;
        else return false;
    }

    vector<int> stack1;
    vector<int> stack2;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```