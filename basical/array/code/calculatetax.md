题目描述：  
![image](/basical/array/image/image39.md)  
解题过程：  
很简单，5分钟ac  
官解写得很精髓，需要记一下，code友说把除以100放在最后处理可以提高效率  
代码：（自己→官解）  
```cpp
class Solution {
public:
    double calculateTax(vector<vector<int>>& brackets, int income) {
        double ans = 0;
        for (int i = 0; i < brackets.size(); i++) {
            double percent = brackets[i][1] * 0.01;
            if (income >= brackets[i][0]) {
                ans += (i == 0) ? brackets[i][0] * percent : (brackets[i][0] - brackets[i-1][0]) * percent;
            } else {
                ans += (i == 0) ? income * percent : (income - brackets[i-1][0]) * percent;
                break;
            }
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    double calculateTax(vector<vector<int>>& brackets, int income) {
        double totalTax = 0;
        int lower = 0;
        for (auto &bracket: brackets) {
            int upper = bracket[0], percent = bracket[1];
            int tax = (min(income, upper) - lower) * percent;
            totalTax += tax;
            if (income <= upper) {
                break;
            }
            lower = upper;
        }
        return (double)totalTax / 100.0;
    }
};
```