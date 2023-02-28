题目描述：  
![image](/basicaldatastructure/setandmap/image/image8.png)  
解题过程：  
哈希表5分钟解决  
题解算法要点：  
1. 使用map（虽然我也想到了，但是我感觉没必要）
2. 对两个item数组进行遍历的时候直接往map中加入元素或者更改元素  
代码：（我的→题解）  
```cpp
class Solution {
public:
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) {
        unordered_map<int,int> map;
        for (auto& item : items2) {
            map[item[0]] = item[1];
        }
        vector<vector<int>> ret;
        for (auto& item : items1) {
            if (map.count(item[0])) {
                ret.push_back({item[0],item[1] + map[item[0]]});
                map.erase(item[0]);
            } else ret.push_back({item[0],item[1]});
        }
        for (auto& [value,weight] : map) {
            ret.push_back({value,weight});
        }
        sort(ret.begin(),ret.end(),[&](vector<int> a,vector<int> b){return a[0] < b[0];});
        return ret;
    }
};
```  
```cpp
class Solution {
public:
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) {
        map<int, int> mp;
        for (auto &v : items1) {
            mp[v[0]] += v[1];
        }
        for (auto &v : items2) {
            mp[v[0]] += v[1];
        }

        vector<vector<int>> res;
        for (auto &[k, v] : mp) {
            res.push_back({k, v});
        }
        return res;
    }
};
```