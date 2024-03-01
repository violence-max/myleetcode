题目描述：  
![image](/basicaldatastructure/stackandquere/image/image20.png)  
解决过程：  
没做出来  
代码：数组+有序集合  
```cpp
class DinnerPlates {
public:
    DinnerPlates(int capacity) {
        this->capacity = capacity;
    }
    
    void push(int val) {
        if (poppedPos.empty()) {
            // 如果没有任何一个栈的被指定弹出过栈顶元素
            int pos = stk.size();
            stk.push_back(val);
            if (pos % capacity == 0) {
                top.push_back(0);
            } else {
                top.back()++;
            }
        } else {
            // 先插入之前弹出的元素，记得同时删除poppedPos中对应的元素
            int pos = *poppedPos.begin();
            poppedPos.erase(pos);
            stk[pos] = val;
            top[pos / capacity]++;
        }
    }
    
    int pop() {
        while (!stk.empty() && poppedPos.count(stk.size()-1)) {
            // 栈的最后一个元素起始已经被调用popAtStack()方法删除了
            int pos = *poppedPos.rbegin();
            poppedPos.erase(pos);
            stk.pop_back();
            if (pos % capacity == 0) {
                top.pop_back();
            }
        }
        bool flag = true;
        int val;
        if (stk.empty()) {
            flag = false;
        } else {
            int pos = stk.size() - 1;
            val = stk[pos];
            stk.pop_back();
            top[pos / capacity]--;
        }
        return !flag ? -1 : val;
    }
    
    int popAtStack(int index) {
        if (index >= top.size() || top[index] < 0) return -1;
        int pos = index * capacity + top[index];
        top[index]--;
        int val = stk[pos];
        poppedPos.emplace(pos);
        return val;
    }

private:
    int capacity;
    // 全局栈，第i个栈的栈顶位置为：i * capacity + <第i个栈的元素个数>
    vector<int> stk; 
    vector<int> top; // 记录每个栈的栈顶元素在该栈中的位置，从0开始
    set<int> poppedPos; // 记录被弹出了的元素的位置
};

/**
 * Your DinnerPlates object will be instantiated and called as such:
 * DinnerPlates* obj = new DinnerPlates(capacity);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAtStack(index);
 */
```