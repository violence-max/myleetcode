题目描述：  
![image](/basical/array/image/image67.png)  
解题过程：  
1秒钟通过  
代码：  
```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        return {celsius + 273.15, celsius * 1.80 + 32.00};
    }
};
```