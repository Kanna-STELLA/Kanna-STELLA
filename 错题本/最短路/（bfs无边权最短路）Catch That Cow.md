[Problem - 2717 (hdu.edu.cn)](https://acm.hdu.edu.cn/showproblem.php?pid=2717)
## 我的思路：
当时想的是dp，dfs（深度优先做不了，求解要把所有可能性都遍历完，复杂度不合适）啥的，完全没想到是bfs的最短路。
## 题解思路：
每次行动耗费1时间，问到达某点的最短时间。
那就是边权为1的最短路问题了。



## 代码：
```cpp
//边权为1的bfs最短路，新情景，每次移动时间耗费+1
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdio>
#include <queue>
#include <vector>
using namespace std;
const int N = 1e5 + 10;
int n,k;

queue<int> q;
int bfs(){
    vector<int> dis(N+1,-1);
	q.push(n);
	dis[n] = 0;
	while(q.size()){
		int t = q.front();
		q.pop();
		for(auto i : vector<int>{1,-1,t}){
			int x = t + i;
			if(x < 0 || x >= N ) continue;
			if(dis[x] != -1) continue;
			q.push(x);
			dis[x] = dis[t] + 1;
			if(x == k) return dis[x];
		}
	}
}
int main(){
	
	while(cin >> n >> k){
		while(q.size()) q.pop(); 
		if(n >= k) cout << n-k << endl;
		else cout << bfs() << endl;
	}
	return 0;
}
```

## 套路：
为什么不用dp？1e5的复杂度，不合适。就算一维dp也是需要两重循环的

### 1）bfs无边权最短路
#### 前提条件:求最短路，无边权图。
#### 情景：
1）Catch That Cow
* Walking: FJ can move from any point X to the points X - 1 or X + 1 in a single minute  
* Teleporting: FJ can move from any point X to the point 2 × X in a single minute.
* 每次移动，时间加一。时间也可以当作是一种距离。
2）nearest Black Vertex
-   At least one vertex is painted black.
-   For every i=1,2,…,K, the following holds:
    -   the minimum distance between vertex pi​ and a vertex painted black is exactly di​.
    - 把不符合要求的棋子标记为白，然后找出每个白棋子距离最近的黑棋子的距离。

都是需要找一个边权为1的图力点之间的最短距离。
## 应对：
### 1）求一个点到n个点的最短路：
把这个点当作起点。
### 2） 求1种点到距离另一种点的最短距离
把其中一种点全部推入循环，然后dis设置为0.n个起点。

#bfs无边权最短路 
