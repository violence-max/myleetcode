题目描述： 
![image](/basical/string/image/image21.png) 
解题过程： 
这个题相当恶心，一直在调bug和修改思路。修改到最后我发现了这个题的一个暴力解法：用集合记录word中的索引，当集合的size和word的一样大时意味着找到一个满足条件的子串。记录每一个子串一开始的索引，扫描到一个单词就添加进集合，然后开始扫描下一个单词，如果扫描不到，那就从上一次开始扫描的位置开始重新扫描，如果得到一个完整的串联子串，那么就记录答案并且从上一次开始扫描的位置的下一个位置继续扫描 
代码：  
暴力： 
```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_set<string> set;
        int m = words.size(),n = words[0].size();
        unordered_set<int> visit;
        for (string s : words) {
            set.insert(s);
        }
        vector<int> ans;
        int i = 0,start = 0;
        while (i < (int)s.size()) {
            string tmp = s.substr(i,n);
            if (set.find(tmp) != set.end()) {
                if (visit.size() == 0) {
                    start = i;
                }
                int visitFlag = false;
                for (int j = 0; j < m; j++) {
                    if (words[j] == tmp && (visit.find(j) == visit.end())) {
                        visit.insert(j);
                        visitFlag = true;
                        break;
                    }
                }
                if (!visitFlag) {
                    while (start < (int)s.size()) {
                        string del = s.substr(start,n);
                        if (del == tmp) break;
                        else start += n;
                    }
                    i = start + 1;
                    visit.clear();
                    continue;
                } else if (visit.size() == m) {
                    ans.push_back(start);
                    i = start + 1;
                    visit.clear();
                    continue;
                }
                i += n;
            } else {
                i = (visit.size() > 0) ? start + 1 : (i + 1);
                visit.clear();
            }
        }
        return ans;
    }
};
```  
滑动窗口正宗版本： 
题解这个和我的真的有很大差别，我是一个字符一个字符查找，并且找到了或者没有找到都要从上一个起始位置的下一个位置开始重新查找，但是题解是真的滑动窗口，看i的起始位置就知道很讲究，以小于n的最大整数为最大值，以集合是否为空为判断条件，每次右边进一个，左边出一个，简直就是梦想中的滑动窗口，奈何我想不出来，而且暂时没有那么多总结的思路。  
```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        int m = words.size(),n = words[0].size(),ls = s.size();
        vector<int> res;
        for (int i = 0; i < n && i + m * n  <= ls; i++) {
            unordered_map<string,int> map;
            for (int j = 0; j < m; j++) {
                map[s.substr(i+j*n,n)]++;
            }
            for (string str : words) {
                if (--map[str] == 0) {
                    map.erase(str);
                }
            }
            for (int start = i; start < ls - m * n + 1; start += n) {
                if (start != i) {
                    string front = s.substr(start+(m-1)*n,n);
                    if (++map[front] == 0) {
                        map.erase(front);
                    }
                    string back = s.substr(start - n,n);
                    if (--map[back] == 0) {
                        map.erase(back);
                    }
                }
                if (map.size() == 0) {
                    res.push_back(start);
                }
            }
        }
        return res;
    }
};
```