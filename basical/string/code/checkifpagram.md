题目描述：  
![image](/basical/string/image/image17.png)  
解题过程：  
太简单了这也，就是用一个大小为26的容器装26个字母而已。不过看了题解了解到了一种新的方法：二进制表示集合，感觉很高级，空间复杂度和时间复杂度都降低了  
代码：  
```cpp
class Solution {
public:
    bool checkIfPangram(string sentence) {
        vector<int> cnt (26);
        for (const auto &c : sentence) {
            cnt[c-'a']++;
        }
        for (auto i : cnt) {
            if (!i) return false;
        }
        return true;
    }
};
```