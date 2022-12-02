题目描述：  
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/890287c1-950e-4350-9a6e-acf1631c2746/Untitled.png)  
解题过程：  
太简单了，一次过，强无敌  
代码：  
```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char,int> map;
        int n = s.size();
        for (int i = 0; i < n; i++) {
            map[s[i]] = i; 
        }
        int index = 0;
        vector<int> ans;
        while (index < n) {
            char c = s[index];
            int rightmost = map[c];
            for (int i = index + 1; i < rightmost; i++) {
                rightmost = (map[s[i]] > rightmost) ? map[s[i]] : rightmost;
                if (rightmost == n - 1) break;
            }
            ans.push_back(rightmost - index + 1);
            index = rightmost + 1;
        }
        return ans;
    }
};
```