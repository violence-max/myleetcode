题目描述：  
![image](/basical/string/image/image69.png)  
解题过程：  
没做出来  
- 双指针：  
字典序最大的子串一定是后缀子串  
代码：  
```cpp
class Solution {
public:
    string lastSubstring(string s) {
        int n = s.size();
        // i指向当前最大字典序的后缀子字符串，j指向待比较的子字符串
        int i = 0, j = 1;
        while (j < n) {
            int k = 0;
            // 跳过中间的一些相等的字符
            while (j + k < n && s[i + k] == s[j + k]) k++;
            if (j + k < n && s[i + k] < s[j + k]) {
                // 如果在k这个位置上待比较的字符串更优，那么请及时更新当前族弟啊字典序的后缀子字符串的起始位置
                int t = i;
                i = j;
                // 分i + k > j 和 i + k <= j两种情况，因此在j + 1和i + k + 1之间取一个最大值作为下一个要比较的后缀子串的起始位置
                // 再强调一遍，i + k + 1是指待比较子串相比现有子串更优的位置
                j = max(j + 1, t + k + 1);
            } else {
                // 现有子串比待比较子串更优
                j = j + k + 1;
            }
        }
        return s.substr(i);
    }
};
```