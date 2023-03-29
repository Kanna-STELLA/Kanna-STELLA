
### 题链
https://www.acwing.com/problem/content/1267/
~~没买课的点不开，耗子尾汁~~ 
文末放图片

### 解决问题先看本质，找数据范围与输出
>// 输出格式
// N行，每行一个整数，分别是 0级，1级，2级，……，N−1级的星星的数目。
// 数据范围
// 1≤N≤15000
// ,
// 0≤x,y≤32000
**范围不是很特殊，没有太多信息**
### // 带着疑惑去看题干，轻松抓重点
==// 级是什么，数量如何统计：==

**在题干中发现：**
>// 如果一个星星的左下方（包含正左和正下）有 k颗星星，就说这颗星星是 k级的。

**//  因为涉及到坐标以及求和，到这里可以想到大概是用一个二维前缀和，但是二维前缀和要开的数组太大了，用不了，再看有没有其他条件，
 //  结果发现还真有:**

>//  **不会有星星重叠。星星按 y坐标增序给出，y坐标相同的按 x坐标增序给出。**

//  给出的点的y是不严格递增的，且y相同时，x是递增的这意味着，前面的点永远在后面的点的下方，

由此题目就变成按顺序看星星的话可以只看x坐标来判断前面的星星是否要计入后面的点的数量。

数量的计算就可以变成计算哈希统计x坐标各种情况的出现次数，然后计算1~x的前缀和。

//  因为涉及到了数组单点修改以及求和操作，所以可以用一个树状数组或线段树来模拟哈希维护数据

//观察样例发现星星计数时是不计自身的，所以先求树状数组里统计的前缀和，再修改树状数组里的值
// 统计数量级又要统计各种数量的出现情况，又要一层哈希

// 第一颗星星的左下方不可能有星星，且计数时不计自身，必然有0级的星星最后遍历哈希0到n-1

---

### 代码：
```cpp
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
using namespace std;
const int N = 4e4;
using namespace std;
int tr[N],w[N],f[N];
int lowbit(int x){
    return x & -x;
}
void add(int x,int v){
    for(int i = x;i <= N;i+=lowbit(i)) tr[i] += v;
}
int query(int x){
    int res = 0;
    for(int i = x;i ;i -=lowbit(i)) res += tr[i];
    return res;
}
int main(){
    int n;
    int x,y;
    cin >> n;
    for(int i = 0;i < n;i++){
        scanf("%d%d",&x,&y);
        x++;
        f[query(x)] ++;
        add(x,1);
    }
    for(int i  = 0;i < n;i++){
        printf("%d\n",f[i]);
    }
    return 0;
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7e15e7c0786e4c6199d6b77bf18da99c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/34dea3e0be2641179a3fb10e0e35e3df.png)

