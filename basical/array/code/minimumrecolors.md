题目描述：  
![image](/basical/array/image/image59.png)  
解决过程：  
两分钟ac，但是因为循环的时候窗口的下标计算出错wa了一次  
代码：  
```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {
        int count = 0, lblock = blocks.size();
        for (int i = 0; i < k; i++) {
            if (blocks[i] == 'W') count++;
        }

        if (count == 0 ) return 0;

        int ret = count;
        for (int i = 1; i <= lblock - k; i++) {
            if (blocks[i-1] == 'W') {
                count--;
            }
            if (blocks[i-1 + k] == 'W') {
                count++;
            }
            ret = min(ret,count);
        }
        return ret;
    }
};
```