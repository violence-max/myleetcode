题目描述：  
![image](/basical/string/image/image61.png)  
解决过程：  
用了哈希表，很慢，是最慢的滑动窗口  
题解思路：用diff来表示s中长度与p相等的子串的字每差异  
1. 一开始将s从索引0开始与p长度相等的子字符串进行字母的比较，得到出现频次不相同的字母个数diff
2. 开始从窗口的左边减去字符和从右边加入字符，同时维护diff
3. diff等于0时说明窗口的子字符串是p的字母异位词  
code友思路：评论区有个code友，给出了常规的滑动窗口的思路  
1. 用hashtable计算p字符串中字符的出现频次
2. 用left和right双指针对字符串s进行滑动。滑动规则为：在hashtable中减去s中下标为right的字母，当该减去的字母在哈希表中体现为出现频次小于0时，就持续从窗口左边还原哈希表
- 这个持续从窗口左边还原哈希表应该值得注意，可以在破坏了哈希表之后将哈希表还原
1. 如果窗口长度与p字符串的长度相同，说明找到了对应的字母异位词  
代码：（哈希表→优化的滑动窗口→常规滑动窗口）  
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int ls = s.size(), lp = p.size();
        if (ls < lp) return {};
        unordered_map<char,int> map;
        for (auto c : p) map[c]++;

        auto check = [&](int start, int end) -> bool {
            unordered_map<char,int> _map = map;
            for (int i = start; i <= end; i++) {
                _map[s[i]]--;
            }
            return !any_of(_map.begin(),_map.end(),[](pair<char,int> p) {return p.second != 0;});
        };

        int start = 0, end = 0;
        vector<int> ret;
        while (end < ls) {
            if (!map.count(s[end])) {
                start = end + 1;
            }
            if (end - start + 1 == lp) {
                if (check(start,end)) ret.push_back(start);
                start++;
            }
            end++;
        }
        return ret;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int ls = s.size(), lp = p.size();
        if (ls < lp) return {};
        vector<int> ans;
        vector<int> hashtable (26);
        for (int i = 0; i < lp; i++) {
            hashtable[s[i] - 'a']++;
            hashtable[p[i] - 'a']--;
        }

        int diff = 0;
        for (int i = 0; i < 26; i++) {
            if (hashtable[i] != 0) diff++;
        }

        if (diff == 0) ans.push_back(0);
        for (int i = 0; i < ls - lp; i++) {
            diff = hashtable[s[i] - 'a'] == 1 ? diff - 1 : 
                   hashtable[s[i] - 'a'] == 0 ? diff + 1 : diff;
            hashtable[s[i] - 'a']--;
            diff = hashtable[s[i + lp] - 'a'] == -1 ? diff - 1 : 
                   hashtable[s[i + lp] - 'a'] == 0 ? diff + 1 : 
                   diff;
            hashtable[s[i + lp] - 'a']++;
            if (diff == 0) ans.push_back(i + 1);
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int ls = s.size();
        vector<int> hashtable (26);
        for (auto c : p) hashtable[c - 'a']++;
        vector<int> ans;
        for (int left = 0, right = 0; right < ls; right++) {
            --hashtable[s[right] - 'a'];
            while (hashtable[s[right] - 'a'] < 0) {
                ++hashtable[s[left++] - 'a'];
            }
            if (right - left + 1 == p.size()) ans.push_back(left);
        }
        return ans;
    }
};
```