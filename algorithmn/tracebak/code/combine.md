题目描述：  
![image](/algorithmn/tracebak/image/image1.png)  
解题过程：  
用的笨办法，以k为1和k为2这两种基本情况为基础，如果k>2，则固定最右边的位置，用一个temp数组两存储除了最右边位置以外其余数字以及k-1过后的答案，计算出来后再加上最右边的数字，将temp数组中的每个值添加到答案数组中。这个方法很慢。  
代码：  
暴力：  
```cpp
class Solution {
public:
    vector<vector<int>> sortArr(int left,int right,int k) {
        vector<vector<int>> res;
        if (k == 1) {
            for (int i = left; i <= right; i++) {
                res.push_back({i});
            }
        } else if (k == 2) {
            while (right > left) {
                vector<int> temp;
                for (int i = left; i < right; i++) {
                    temp = {i,right};
                    res.push_back(temp);
                }
                right--;
            }
        } else {
            while (right > left) {
                vector<vector<int>> temp = sortArr(left,right-1,k-1);
                for (auto& a : temp) {
                    a.push_back(right);
                    res.push_back(a);
                }
                right--;
            }
        }
        return res;
    }
    vector<vector<int>> combine(int n, int k) {
        return sortArr(1,n,k);
    }
};
```  
回溯：  
```cpp
class Solution {
public:
    vector<int> temp;
    vector<vector<int>> ans;

    void dfs(int cur, int n, int k) {
        if (temp.size() + (n - cur + 1) < k) {
            return;
        }
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        temp.push_back(cur);
        dfs(cur + 1, n, k);
        temp.pop_back();
        dfs(cur + 1, n, k);
    }

    vector<vector<int>> combine(int n, int k) {
        dfs(1, n, k);
        return ans;
    }
};
```