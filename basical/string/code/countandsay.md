题目描述：  
![image](/basical/string/image/image24.png)  
解决过程：  
直接一个递归就解决了，一次过，舒胡  
代码：  
```cpp
class Solution {
public:
    void construct(string &ans,int start,int i,string str) {
        int len = i - start;
        ans += to_string(len);
        ans += str[i - 1];
    }

    string countAndSay(int n) {
        if (n == 1) return "1";
        else {
            string str = countAndSay(n - 1);
            int i = 0,ls = (int)str.size();
            int start = i;
            string ans;
            while (i < ls) {
                if (i > 0 && str[i] != str[i - 1]) {
                    construct(ans,start,i,str);
                    start = i;
                }
                i++;
            }
            if (start < i) {
                construct(ans,start,i,str);
            }
            return ans;
        }
    }
};
```