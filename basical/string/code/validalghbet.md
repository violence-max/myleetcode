题目描述：  
![image](/basical/string/image/image3.png)    
解题过程：直接使用哈希表进行解决，但是一开始忘记了哈希表是怎么使用的了（主要还是不够熟悉）。首先是在键值对那一块把char写成了string很搞，其次是不清楚哈希表进行索引了之后直接就是值了，还在那使用结构体想要找出second值，但是那玩意已经不是迭代器了，操。其实这个题目就是把s和t的值做成哈希表，然后对s的哈希表进行遍历，如果t中对应的值不相等则返回false。一开始我做的确实是这样的，但是我忽略了s是t的子集的情况，所以我又做了一遍s的表对t的表的遍历，然后我发现很浪费时间，因为做了很多重复遍历。看了题解我发现可以提前判断两个字符串的长度进行提前返回（我居然没想到）。如果使用这一个点进行优化我觉得我的代码应该能跑得更快吧，虽然我没有试。  
代码：  
```cpp
class Solution {
public:
    unordered_map<char,int> initHashMap(string str) {
        unordered_map <char,int> hashMap;
        for (auto &c : str) {
            hashMap[c]++;
        }
        return hashMap;
    }

    bool check(unordered_map<char,int> fisrtSet,unordered_map<char,int>secondSet) {
        for (auto it : fisrtSet) {
            if (secondSet[it.first] != it.second) {
                return false;
            }
        }
        return true;
    }

    bool isAnagram(string s, string t) {
        unordered_map <char,int> sSet,tSet;
        sSet = initHashMap(s);
        tSet = initHashMap(t);
        return check(sSet,tSet) && check(tSet,sSet);
    }
};
```