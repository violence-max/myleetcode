题目描述：  
![image](/algorithmn/dynamic_programming/image/image11.png)  
解题过程：  
应该是要用到单调栈才对，但是一开始只能想到两次循环  
代码：  
超时版本 --两次for循环  
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> ans (n);
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (temperatures[j] > temperatures[i]) {
                    ans[i] = j - i;
                    break;
                }
            }
        }
        return ans;
    }
};
```  
自己写的栈的辅助结构，时间复杂度O(n)，舒服。一开始用哈希map来存储索引，后面发现这个方法简直傻逼，因为键值对会被相同的键覆盖，看来字节面试官也是个乐色  
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> ans (n);
        stack<int> helper;
        helper.push(0);
        for (int i = 1; i < n; i++) {
            int nowTemp = temperatures[i];
            while (!helper.empty()) {
                int topIndex = helper.top();
                int top = temperatures[topIndex];
                if (nowTemp > top) {
                    ans[topIndex] = i - topIndex;
                    helper.pop();
                    if (helper.empty()) {
                        helper.push(i);
                        break;
                    }
                } else {
                    helper.push(i);
                    break;
                }
            }
        }
        return ans;
    }
};
```