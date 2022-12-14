题目描述：  
![image](/algorithmn/greed/image/image18.png) 
解决过程： 
一开始看到这个题目还以为是个背包问题，盯着这个题目好久还是没有思考出一个行之有效的解法。于是看了题解。  
题解就是很简单的一个贪心的思路：先将a，b，c升序排序，如果a+b的值小于c那么可以用c的这一堆石子把a和b的石子全部匹配掉，得到答案a+b。如果a+b的值大于等于c，那么可以用a或者b中石子比较多的堆先把c的堆给匹配掉，然后再a和b两两配对得到答案：假设a和c配对了i个石子，b和c配对了j个石子，则得到的分数为：i+j+[(a-i)+(b-j)] / 2,这里的除2表示的是向下取整。当a+b大于等于c时，由于排序的原因，a和b最大只能取到c，即a小于等于c且b小于等于c，在一种比较合理的游戏方法里，应该使得在将c的堆匹配完了之后a和b的堆的石子的个数尽量相等，事实是，如果总是选取a和b中石子个数较大的那一方进行匹配，当c中的石子匹配完成之后a和b中堆的石子个数相等或者相差1，因此才出现了a和b余下的石子的数量之和对2向下取整那一幕。而a和b将c匹配完之后各自石子个数相等或者相差1很好理解：先选取a和b中石子数量较多的那一堆和c进行匹配，当这一堆石子的个数和石子个数较小的那一堆相等时，再由两个堆进行轮流地对c进行匹配。而我要说地是对这种情况下答案的一个简化：因为i+j和c相等，所以答案可以表示为：c+[(a+b-c)]/2，，即（a+b+c）/ 2 
代码： 
```cpp
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        int sum = a + b + c;
        int maxval = max({a,b,c});
        if (sum - maxval < maxval) {
            return sum - maxval;
        } else {
            return sum / 2;
        }
    }
};
```