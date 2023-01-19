题目描述：  
![image](/basical/string/image/image4.png)  
解题过程：  
这个题太简单了，不想说了，就是建两个哈希表然后比较一下次数的问题，跟按个字母异位词差不多，或者说一模一样。唯一值得注意的是官方的解法，先是用长度判读进行提前处理，然后是用一个额外的数组记录字母出现的次数，记录的方式也很新颖，用的是一个api：toCharArray()和ASCLL字母相减法。  
代码：  
```cpp
class Solution {
public:
    unordered_map<char,int> buildHashMap(string str) {
        unordered_map<char,int> hashMap;
        for (auto & c : str) {
            ++hashMap[c];
        }
        return hashMap;
    }
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int> ransomNoteHashMap = buildHashMap(ransomNote),magazineHashMap = buildHashMap(magazine);
        for (auto it : ransomNoteHashMap) {
            if (magazineHashMap[it.first] < it.second) {
                return false;
            }
        }
        return true;
    }
};
```