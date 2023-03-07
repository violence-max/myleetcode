题目描述：  
![image](/basical/string/image/image62.png)  
解决过程：  
没有思路  
题解的两个思路：递归解析→栈  
- 递归解析：分三个状态：expr，term和iterm
- 栈：当遇到逗号时需要向判断乘号的例子：{{a,b}{c,d},a}，最后一个逗号需要先计算乘号  
代码：（递归解析→栈）  
```cpp
class Solution {
string expression;
int idx;
public:
    set<string> item() {
        set<string> ret;
        if (expression[idx] == '{') {
            idx++;
            ret = expr();
        } else {
            string str (1,expression[idx]);
            ret.insert(str);
            // cout << idx << ' ' << str << endl;
        }
        idx++;
        return move(ret);
    }

    set<string> term() {
        set<string> ret = {""};
        while (idx < expression.size() && (expression[idx] == '{' || isalpha(expression[idx]))) {
            auto sub = item();
            set<string> temp;
            for (auto& left : ret) {
                for (auto& right : sub) {
                    temp.insert(left + right);
                }
            }
            ret = move(temp);
        }
        return move(ret);
    }

    set<string> expr() {
        set<string> ret;
        while (true) {
            ret.merge(term());
            if (idx < expression.size() && expression[idx] == ',') {
                idx++;
                continue;
            } else break;
        }
        return ret;
    }

    vector<string> braceExpansionII(string expression) {
        this->expression = expression;
        this->idx = 0;
        auto ret = expr();
        return {ret.begin(), ret.end()};
    }
};
```  
```cpp
class Solution {
public:
    vector<string> braceExpansionII(string expression) {
        vector<char> op;
        vector<set<string>> stk;

        // 弹出栈顶运算符，并进行计算
        auto ope = [&]() {
            int l = stk.size() - 2, r = stk.size() - 1;
            if (op.back() == '+') {
                stk[l].merge(stk[r]);
            } else {
                set<string> tmp;
                for (auto &left : stk[l]) {
                    for (auto &right : stk[r]) {
                        tmp.insert(left + right);
                    }
                }
                stk[l] = move(tmp);
            }
            op.pop_back();
            stk.pop_back();
        };

        for (int i = 0; i < expression.size(); i++) {
            if (expression[i] == ',') {
                // 不断地弹出栈顶运算符，直到栈为空或者栈顶不为乘号
                while (op.size() && op.back() == '*') {
                    ope();
                }
                op.push_back('+');
            } else if (expression[i] == '{') {
                // 首先判断是否需要添加乘号，再将 { 添加到运算符栈中
                if (i > 0 && (expression[i - 1] == '}' || isalpha(expression[i - 1]))) {
                    op.push_back('*');
                }
                op.push_back('{');
            } else if (expression[i] == '}') {
                // 不断地弹出栈顶运算符，直到栈顶为 {
                while (op.size() && op.back() != '{') {
                    ope();
                }
                op.pop_back();
            } else {
                // 首先判断是否需要添加乘号，再将新构造的集合添加到集合栈中
                if (i > 0 && (expression[i - 1] == '}' || isalpha(expression[i - 1]))) {
                    op.push_back('*');
                }
                stk.push_back({string(1, expression[i])});
            }
        }
        
        while (op.size()) {
            ope();
        }
        return {stk.back().begin(), stk.back().end()};
    }
};
```