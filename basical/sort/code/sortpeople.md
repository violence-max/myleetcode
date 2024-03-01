题目描述：  
![image](/basical/sort/image/image4.png)  
解题过程：  
2分钟ac  
- 要点：height中每个值互不相同  
代码：我的→题解  
```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        int n = names.size();
        unordered_map<int, string> map;
        for (int i = 0; i < n; i++) {
            map[heights[i]] = names[i];
        }
        sort(heights.begin(), heights.end(), greater<int>());
        for (int i = 0; i < n; i++) {
            names[i] = map[heights[i]];
        }
        return names;
    }
};
```  
```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        int n = names.size();
        vector<int> indices(n);
        iota(indices.begin(), indices.end(), 0);
        sort(indices.begin(), indices.end(), [&](int x, int y) {
            return heights[x] > heights[y];
        });
        vector<string> res(n);
        for (int i = 0; i < n; i++) {
            res[i] = names[indices[i]];
        }
        return res;
    }
};
```