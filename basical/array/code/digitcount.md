题目描述：  
![image](/basical/array/image/image36.png) 
解题过程：  
用了两个哈希表，一个哈希表统计数组中数字出现的频次，一个数字记录数位上的目标值，然后两次遍历target哈希表，判断目标值和频次哈希表中的值是否相等。  
code友有一个我觉得很有趣的方法：用数组长度n初始化一个值全部为’0’的字符串s，然后一次遍历字符串num，用s代替哈希表，若num中数字为1则在s的1位置上加1，最后比较s和num是否相等。相等意味着num中每个数位上的目标刚好符合情况。  
一开始以为num是一个容器来着，我说怎么不通过，后来想要debug的时候发现居然是字符串。  
代码：  
```cpp
class Solution {
public:
    bool digitCount(string num) {
        unordered_map<int,int> cnt,target;
        for (int i = 0; i < num.size(); i++) {
            cnt[num[i]-'0']++;
            target[i] = num[i] - '0';
        }
        for (auto &t : target) {
            if (cnt[t.first] != t.second) {
                return false;
            }
        }
        return true;
    }
};
```