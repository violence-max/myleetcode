题目描述：  
![image](/basical/string/image/image2.png)
解题过程：  
典型的滑动窗口问题，我在做这个题的时候遇到了几个困难：  
1. 不知道c++如何获取一个字符串中的任意一个字符（发现像python一样可以视作数组）
2. 不知道如何获取子串（不知道substr这个api）
3. 对于原始的目标字符串哈希表和动态的哈希表的比较不太清楚
4. 对于整个窗口的滑动过程不是特别理解  
滑动窗口，先找到满足目标的一个字符串下标，然后删除左边，右指针继续滑动，就结束了。  
代码：  
```cpp
class Solution {
public:
    unordered_map<char,int> tMap,sMap;

    bool check() {
        for (auto c : tMap) {
            if (sMap[c.first] < c.second) {
                return false;
            }
        }
        return true;
    }

    string minWindow(string s, string t) {
        for (auto c : t) {
            ++tMap[c];
        }
        int left = 0,right = -1;
        int result = INT_MAX,length = s.size(),ansStart = 0;
        while (right < length) {
            if (tMap.find(s[++right]) != tMap.end()) {
                sMap[s[right]]++;
            }
            while (check() && left <= right) {
                if (right - left + 1 < result) {
                    result = right - left + 1;
                    ansStart = left;
                }
                if (sMap.find(s[left]) != sMap.end()) {
                    sMap[s[left]]--;
                }
                left++;
            }
        }
        return (result == INT_MAX) ? string() : s.substr(ansStart,result);
    }
};
```