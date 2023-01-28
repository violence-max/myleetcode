题目描述：  
![image](/basical/string/image/image52.png)  
解决过程：  
靠，一开始就想的是用栈去分割句点，恶心死你叔叔了  
题解很简洁  
代码：（栈分割→双指针）  
```cpp
class Solution {
public:
    void excute(stack<int>& stk, const string& version) {
        int end = version.size() - 1;
        while (end >= 0) {
            int start = end;
            while (start >= 0 && version[start] != '.') start--;
            stk.push(stoi(version.substr(start+1,end-start),0,10));
            end = start - 1;
        }
    }
    bool get(stack<int>& stk) {
        while (!stk.empty()) {
            int num = stk.top(); stk.pop();
            if (num != 0) return true;
        }
        return false;
    }
    int compareVersion(string version1, string version2) {
        stack<int> stk_1, stk_2;
        excute(stk_1,version1);
        excute(stk_2,version2);
        while (!stk_1.empty() && !stk_2.empty()) {
            int num1 = stk_1.top(); stk_1.pop();
            int num2 = stk_2.top(); stk_2.pop();
            if (num1 == num2) continue;
            else if (num1 > num2) return 1;
            else return -1;
        }
        if (!stk_1.empty() && get(stk_1)) return 1;
        if (!stk_2.empty() && get(stk_2)) return -1;
        return 0;  
    }
};
```
```cpp
class Solution {
public:
    int compareVersion(string version1, string version2) {
        int n = version1.length(), m = version2.length();
        int i = 0, j = 0;
        while (i < n || j < m) {
            int x = 0;
            for (; i < n && version1[i] != '.'; ++i) {
                x = x * 10 + (version1[i] - '0');
            }
            ++i;
            int y = 0;
            for (; j < m && version2[j] != '.'; ++j) {
                y = y * 10 + (version2[j] - '0');
            }
            ++j; 
            if (x != y) {
                return x > y ? 1 : -1;
            }
        }
        return 0;
    }
};
```