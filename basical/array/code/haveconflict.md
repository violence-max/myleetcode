题目描述：  
![image](/basical/array/image/image72.png)  
解决过程：  
- HH:MM的时间格式是可以直接使用字符串比较的  
代码：（我的→题解）  
```cpp
class Solution {
public:
    int tominute(string& str) {
        return 60 * stoi(str.substr(0,2)) + stoi(str.substr(3,2));
    }

    bool haveConflict(vector<string>& event1, vector<string>& event2) {
        int start1 = tominute(event1[0]), start2 = tominute(event2[0]);
        int end1 = tominute(event1[1]), end2 = tominute(event2[1]);
        return (start1 <= start2 && start2 <= end1) || (start2 <= start1 && start1 <= end2);
    }
};
```
```cpp
class Solution {
public:
    bool haveConflict(vector<string>& event1, vector<string>& event2) {
        return !(event1[1] < event2[0] || event2[1] < event1[0]);
    }
};
```