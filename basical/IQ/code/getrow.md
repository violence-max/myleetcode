题目描述：  
![image](/basical/IQ/image/image17.png)  
解决过程：  
就是I的递归版本，但是题解给出了各种做法，其中有一种做法比较抽象，这里记录一下：  
代码：（递归→迭代）  
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        if (rowIndex == 0) return {1};
        vector<int> prev = getRow(rowIndex - 1);
        vector<int> ans (prev.size() + 1 , 1);
        for (int i = 1; i < ans.size() - 1; i++) {
            ans[i] = prev[i-1] + prev[i];
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> row(rowIndex + 1);
        row[0] = 1;
        for (int i = 1; i <= rowIndex; ++i) {
            for (int j = i; j > 0; --j) {
                row[j] += row[j - 1];
            }
        }
        return row;
    }
};
```