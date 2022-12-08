题目描述：  
![image](/algorithmn/dynamic_programming/image/image30.png)  
解题过程：  
简单，就是双指针结束，一开始看错题目了，以为是连续的子序列，不过瞬间发现就改过来了  
代码：  
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int m = t.size(),n = s.size();
        if (m < n) return false;
        int k = 0;
        for (int i = 0; i < m; i++) {
            if (s[k] == t[i]) {
                k++;
            }
        }
        if (k == n) return true;
        else return false;
    }
};
```