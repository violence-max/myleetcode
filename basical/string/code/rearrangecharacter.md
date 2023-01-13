题目描述：  
![image](/basical/string/image/image40.png)  
解决过程：  
因为昨天写扰乱字符串的时候学会了判断两个字符串中是否字符出现频次相同的算法，今天就想挑战只用一个哈希表解决，但是发现不太行，无奈之下就用了两个哈希表三次循环  
代码：  
```cpp
class Solution {
public:
    int rearrangeCharacters(string s, string target) {
        unordered_map<char,int> map,freq;
        for (char &c : target) {
            map[c]++;
        }
        for (char &c : s) {
            if (map.count(c)) {
                freq[c]++;
            }
        }
        int ans = INT_MAX;
        for (auto &v : map) {
            ans = min(ans,freq[v.first] / v.second);
        }
        return ans;
    }
};
```