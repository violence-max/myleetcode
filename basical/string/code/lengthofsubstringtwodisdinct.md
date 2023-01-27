题目描述：  
![image](/basical/string/image/image50.png)  
解决过程：  
滑动窗口，我艹了，没有想到我对这类问题已经不熟悉了，一开始用的哈希set不是哈希map  
代码：  
```cpp
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        int ans = INT_MIN;
        int left = 0, right = 0, n = s.length();
        unordered_map<char,int> map;
        while (right < n) {
            map[s[right]]++;
            if (map.size() > 2) {
                ans = max(ans,right - left);
                while (map.size() > 2) {
                    char c = s[left++];
                    map[c]--;
                    if (map[c] == 0) {
                        map.erase(c);
                    }
                }
            }
            right++;
        }
        ans = max(ans,right - left);
        return ans;
    }
};
```