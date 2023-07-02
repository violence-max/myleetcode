题目描述：

![image](/basical/array/image/image85.png)

自定义排序没做出来

- 思维：统计同一纵坐标的所有点，在矩形数组里面寻找所有能包含该y值的矩形，并且按照x轴升序排序，结合二分法求出每个点所包含的矩形数目

代码：

```cpp
class Solution {
public:
    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) {
        int n = points.size();
        vector<int> ans (n);
        unordered_map<int, vector<pair<int, int>>> map;
        for (int i = 0; i < n; ++i) {
            map[points[i][1]].push_back({points[i][0], i});
        }
        for (const auto& [y, list] : map) {
            vector<int> x;
            for (const auto& rectangle : rectangles) {
                if (rectangle[1] >= y) {
                    x.push_back(rectangle[0]);
                }
            }
            sort(x.begin(), x.end());
            for (const auto& p : list) {
                int xx = p.first, idx = p.second;
                ans[idx] = x.end() - lower_bound(x.begin(), x.end(), xx);
            }
        }
        return ans;
    }
};
```