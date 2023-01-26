题目描述：  
![image](/basicaldatastructure/stackandquere/image/image17.png)   
解题过程：  
方法一是使用辅助空间存储最小值，同步压入和弹出；  
方法二是使用与最小值的差值来进行压入和弹出  
代码：（辅助栈→差值）  
```cpp
class MinStack {
private:
    stack<int> s;
    stack<int> min_s;
public:
    MinStack() {
        min_s.push(INT_MAX);
    }
    
    void push(int val) {
        s.push(val);
        min_s.push(min(min_s.top(),val));
    }
    
    void pop() {
        s.pop(); min_s.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return min_s.top();
    }
};
```
```cpp
class MinStack {
private:
    long MIN;
    stack<long long> stk;
public:
    MinStack() {}
    
    void push(int val) {
        if (stk.empty()) {
            stk.push(0);
            MIN = (long)val;
        } else {
            long long diff = (long long)val - MIN;
            stk.push(diff);
            MIN = min(MIN,(long)val);
        }
    }
    
    void pop() {
        long long diff = stk.top();
        if (diff < 0) {
            MIN -= (long)diff;
        }
        stk.pop();
    }
    
    int top() {
        long long diff = stk.top();
        if (diff >= 0) {
            return (int) (MIN + diff);
        } else {
            return MIN;
        }
    }
    
    int getMin() {
        return MIN;
    }
};
```