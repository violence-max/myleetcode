题目描述：  
![image](/basical/IQ/image/image25.png)  
解决过程：  
不到5分钟解决战斗  
代码:  
```cpp
class Solution {
public:
    string bestHand(vector<int>& ranks, vector<char>& suits) {
        unordered_map<int,char> cnt;
        unordered_set<char> set;
        for (int i = 0; i < ranks.size(); i++) {
            cnt[ranks[i]]++;
            set.insert(suits[i]);
        }   
        if (set.size() == 1) return "Flush";
        else if (any_of(cnt.begin(),cnt.end(),[](auto t) {return t.second >= 3;})) return "Three of a Kind";
        else if (cnt.size() <= 4) return "Pair";
        else return "High Card"; 
    }
};
```