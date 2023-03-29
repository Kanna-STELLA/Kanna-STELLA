~~做题时和其他题混淆了，用了枚举日志的方法，只维护了相邻的两个点赞的信息~~

>acwing外卖店优先级
>https://www.acwing.com/problem/content/description/1243/ 
>pta银行排队问题之单队列多窗口服务
>https://pintia.cn/problem-sets/1635264548121292800/exam/problems/1636695635989049353

**三者的共同点**是题目输入都给了一个时间和id的二元组
且看题干都是要遍历一遍这个二元组的

**不同的点**是日志统计这个题，给出一个固定的区间，赞的间隔要在这个区间内才算数，右边界更新后，左边的数不能抛弃不管，需要记录在这个区间左右边界的赞的时间，要有个指针指向左边界并且根据右边界的移动而不断移动，恰巧就符合双指针滑动窗口的特征
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c1ef43f86ab48ac9c8860b372a20d2c.png)
而外卖店优先级对订单（“赞”）的间隔是没有一个固定的要求的，只需要记录前面的订单（赞）给后面遗留了一个什么样的状态，根据两个相邻订单a,b(“赞”）的时间间隔来更新这个状态，并遗留给下一个订单c（“赞”），遗留过后，a就对后面的订单没有任何影响，可以彻底抛弃不管了
![在这里插入图片描述](https://img-blog.csdnimg.cn/d8ac831cd4484c25afca0381bfc8bcf9.png)
#### 二刷卡壳，最后输出时i的范围写错了，没有走流程对比题干


代码
```cpp
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
using namespace std;
const int N  = 1e5 + 10;
int t[N];
struct Tiezi{
    int ts,id;
    bool operator<(Tiezi w){
        return ts < w.ts;
    }
}tiezi[N];
bool st[N];
int cnt[N];
int main(){
    int n,d,k;
    cin >> n >> d >> k;
    int ts,id;
    for(int i= 0;i < n;i++){
        cin >> ts >> id;
        tiezi[i] = {ts,id};
        
    }
    sort(tiezi,tiezi+n);
    
    for(int i = 0,j = 0;j < n;j++){
        // 加的是j指针的帖子，所以j要从0开始
        int id = tiezi[j].id;
        int ts = tiezi[j].ts;
        
        cnt[id]++;
        while(ts - tiezi[i].ts >= d){
            cnt[tiezi[i].id] -= 1;
            i++;
            
        }
        if(cnt[id] >= k) st[id] = true;
        // cout << cnt[1] << endl;
    }
    for(int i = 0;i < N;i++){
        if(st[i]) cout << i << endl;
    }
    return 0;
}
```
