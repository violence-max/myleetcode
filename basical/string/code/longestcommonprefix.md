题目描述：  
![image](/basical/string/image/image20.png)  
解决过程： 
按照题解的说法，我这个方法叫做横向查找。因为我发现最长前缀不会边长只会变短，所以我用0和1字符串作为最开始的参数先计算出一个最长前缀，然后再往后逐渐减。  
题解还有纵向查找、二分查找和分治等思想  
代码：  
```cpp
class Solution {
public:
    int cmp(string prefix,string pattern) {
        int pl = prefix.size(),l = pattern.size();
        int index1 = 0,index2 = 0;
        while (index1 < pl && index2 < l) {
            if (prefix[index1] == pattern[index2]) {
                index1++;
                index2++;
            } else {
                break;
            }
        }
        return index1;
    }
    string longestCommonPrefix(vector<string>& strs) {
        int n = strs.size();
        if (n == 1) return strs[0];
        string prefix = strs[0].substr(0,(cmp(strs[0],strs[1])));
        for (int i = 2; i < n; i++) {
            prefix = prefix.substr(0,(cmp(prefix,strs[i])));
            if (!prefix.size()) break;
        }
        return prefix;
    } 
};
```