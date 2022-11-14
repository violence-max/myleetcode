题目描述：  
![image](/basicaldatastructure/stackandquere/image/image4.png)  
解题过程：  
这个题目跟用栈实现队列基本没有什么区别，就是用个队列而已。比较困难的是如何用一个队列去实现栈，看了一下题解，就是出队然后再加到队尾哈哈哈。  
代码：  
```cpp
class MyStack {
private:
    vector<int> quere1;
    vector<int> quere2;
public:
    MyStack() {}
    
    void push(int x) {
        quere1.insert(quere1.begin(),x);
    }
    
    int pop() {
        int result = top();
        quere1.erase(quere1.begin());
        while (quere2.size()) {
            quere1.insert(quere1.begin(),quere2.back());
            quere2.erase(quere2.end() - 1);
        }
        return result;
    }
    
    int top() {
        while (quere1.size() > 1) {
            quere2.insert(quere2.begin(),quere1.back());
            quere1.erase(quere1.end() - 1);
        }
        return quere1.at(0);
    }
    
    bool empty() {
        if (quere1.size() == 0 && quere2.size() == 0) return true;
        else return false;
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```