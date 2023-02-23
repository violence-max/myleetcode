题目描述：  
![image](/basical/IQ/image/image27.png)  
解题过程:  
真心不大会，看了题解，枚举、二分查找和双指针（跟二维矩阵搜索的思想类似）  
算法要点:  
1. 只需要注意单调性和x与y的范围
2. 注意可以直接尝试使用方法的调用，即f(x,y)  
代码：（双指针→枚举→二分查找）  
```cpp
class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& customfunction, int z) {
       vector<vector<int>> res;
       for (int x = 1, y = 1000; x <= 1000 && y >= 1; x++) {
           while (y >= 1 && customfunction.f(x,y) > z) {
               y--;
           }
           if (y >= 1 && customfunction.f(x,y) == z) {
               res.push_back({x,y});
           }
       } 
       return res;
    }
};
```  
```cpp
class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& customfunction, int z) {
        vector<vector<int>> res;
        for (int x = 1; x <= 1000; x++) {
            for (int y = 1; y <= 1000; y++) {
                if (customfunction.f(x, y) == z) {
                    res.push_back({x, y});
                }
            }
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& customfunction, int z) {
        vector<vector<int>> res;
        for (int x = 1; x <= 1000; x++) {
            int yleft = 1, yright = 1000;
            while (yleft <= yright) {
                int ymiddle = (yleft + yright) / 2;
                if (customfunction.f(x, ymiddle) == z) {
                    res.push_back({x, ymiddle});
                    break;
                }
                if (customfunction.f(x, ymiddle) > z) {
                    yright = ymiddle - 1;
                } else {
                    yleft = ymiddle + 1;
                }
            }
        }
        return res;
    }
};
```