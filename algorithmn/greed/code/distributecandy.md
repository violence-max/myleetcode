题目描述：  
![image](/algorithmn/greed/image/image8.png) 
解题过程：  
尝试了排序后遇到大的值直接总数加一，后面发现有相邻之间的要求，直接死掉。  
看了题解，第一种方法采取左规则和右规则两次遍历，第二种方法采取常数空间的升序和降序取巧，都写了。  
代码：  
左规则和右规则：  
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int length = ratings.size();
        vector<int> left(length);
        left[0] = 1;
        for (int i = 0; i < length; i++) {
            if (i > 0 && ratings[i] > ratings[i-1]) {
                left[i] = left[i-1] + 1;
            } else left[i] = 1;
        }
        int ret = 0;
        int right = 1;
        for (int i = length - 1; i >= 0; i--) {
            if (i < length - 1 && ratings[i] > ratings[i+1]) {
                right = right + 1;
            } else right = 1;
            ret += max(right,left[i]);
        }
        return ret;
    }
};
```  
升序和降序取巧：  
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        int ret = 1;
        int inc = 1,pre = 1,dec = 0;
        for (int i = 1; i < n; i++) {
            if (ratings[i] >= ratings[i-1]) {
                dec = 0;
                pre = ratings[i] == ratings[i-1] ? 1 : pre + 1;
                ret += pre;
                inc = pre;
            } else {
                dec++;
                if (dec == inc) {
                    dec++;
                }
                ret += dec;
                pre = 1;
            }
        }
        return ret;
    }
};
```