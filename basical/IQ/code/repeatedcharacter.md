题目描述：  
![image](/basical/IQ/image/image13.png)  
解题过程：  
一个哈希表等于强无敌  
代码：（哈希表→位运算）  
```cpp
class Solution {
public:
    char repeatedCharacter(string s) {
        unordered_set<char> set;
        char ans = '0';
        for (auto& c : s) {
            if (set.find(c) != set.end()) {
                ans = c;
                break;
            } else {
                set.insert(c);
            }
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    char repeatedCharacter(string s) {
        char ans = '0';
        int check = 0;
        for (auto& c : s) {
            int x = c - 'a';
            if (check & (1 << x)) {
                ans = c;
                break;
            } else {
                check = check | (1 << x);
            }
        }
        return ans;
    }
};
```