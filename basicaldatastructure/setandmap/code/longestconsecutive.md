题目描述：  
![image](/basicaldatastructure/setandmap/image/image4.png)  
解题过程：  
很简单，我用的set，虽然复杂度是nlogn，但是思路很简洁（一开始迭代器在走，origin不变让我还debug了以下，很难受）  
看了题解，题解的方法也挺有意思的，哈希表解决，因为哈希表的find和insert的时间复杂度可以接近于O(1)，还是挺不错的。  
代码（set→unordered_set）  
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        set<int> s;
        for (auto& i : nums) {
            s.insert(i);
        }
        int ans = 0;
        set<int>::iterator it = s.begin();
        while (it != s.end()) {
            int result = 1;
            int origin = *it;
            it++;
            while (it != s.end() && (*it) == origin + 1) {
                result++;
                origin = *it;
                it++;
            }
            ans = max(ans,result);
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> set;
        for (int& i : nums) {
            set.insert(i);
        }
        int longsetstreak = 0;
        for (const int& i : set) {
            if (!set.count(i-1)) {
                int length = 1;
                int currnum = i;
                while (set.count(currnum+1)) {
                    currnum += 1;
                    length++;
                }
                longsetstreak = max(longsetstreak,length);
            }
        }
        return longsetstreak;
    }
};
```