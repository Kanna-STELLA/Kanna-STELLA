[E - Nearest Black Vertex (atcoder.jp)](https://atcoder.jp/contests/abc299/tasks/abc299_e)

### 题面
如果无法将每个顶点绘制为黑色或白色以满足条件，请打印 `No`.否则，打印 `Yes`


如果顶点 i is painted black and 被漆成黑色 00 if white. 如果是白色。


第一行是否有解，YES NO
第二行字符串1是黑，0是白。
连通无向图 不包含自环和多边

第i条边链接ui和vi，
至少有一个顶点绘制为黑色。
pi和黑色点的最小距离是di


u和v的距离是u和v路径上的最小边数量



无向图 不包含自环和多边
至少有一个顶点绘制为黑色。
pi和黑色点的最小距离是di
边权为1

黑色点特殊点，先假设一个点是特殊点，再跑下最短路，看是否满足所有的d条件。
但是这样不一定可以确定一个特殊点，d可能需要多个特殊点同时存在才能满足。

有解情况没想法。

无解情况，只有一个点时，如果d！= 0无解
两个点，如果d ！= 1或0，无解
3个点，d要>=0 <= 2,
***
所以d要>= 0,<=  n-1;


### 题解思路：

用bfs搜点，< d的标记为白
再搜一次，如果不存在>= d的就NO，用bool数组记录情况，最后一边遍历一边输出就可以实现用所谓的“string”来表示点的信息。

题目问的棋子是否符合要求：
首先是需要得到黑白棋子的情况。

白棋和黑棋两个对立面，可以随便选一个来操作。
这里选白棋来接受操作。

要知道哪个可以作黑棋很难，这时可以反着想，
哪个不能做黑棋？题目对黑棋做了什么限制？

给了k个棋子，要求与这些棋子距离最近的黑棋子的距离为di，所以
距离这些棋子的距离小于di的是不能作为白棋子的，全部标记为黑棋子，其余标记为白棋子，这样就得到了黑白情况了。

然后，题目要距离白棋子最近的黑棋子距离=di，再处理每一个棋子和黑棋子的最短距离。
对应求一种点到另一种点的最短距离套路，看距离是否=di就行了。不符合输出no然后return
全部处理完后再输出yes

#### 代码与分析
##### jls代码
```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
long long 简写

int main() {

    int N, M;
    cin >> N >> M;
    ***
    vector<vector<int>> adj(N);
    还以为是什么函数，其实是数组名
    for (int i = 0; i < M; i++) {
        int u, v;
        cin >> u >> v;
        u--, v--;
        vector下标从0开始，uv是1开始，所以u--，v--
        adj[u].push_back(v);
        adj[v].push_back(u);
                权值都为1，没必要储存，所以第二维直接pb，遍历时可以用迭代，不用把整个N都遍历，

	}
    ***
    vector<bool> black(N, true);
    大小为N，初始值为true的vector
    int K;
    cin >> K;
    
    vector<int> p(K), d(K);
    大小为k的p，d vector
    for (int i = 0; i < K; i++) {
        cin >> p[i] >> d[i];
        p[i]--;
        统一下标
            跑k次dfs，以i为起点，bfs，就知道各个点离i的距离，用距离来和d[i]比较，可以把不符合要求的点全部设为白。  但如果是求距离的话为什么不用最短路？
    bfs，一边处理，一边就可以判断点。
    最短路：
    跑完一遍最短路之后，要遍历其他点，判是否距离<d[i],好像也不是不行。
    不太知道bfs的前提，需要做做这个专题，就这题来看，因为要知道所有点到当前点的距离，所以挺适合bfs。
    ***
        vector<int> dis(N, -1);
        大小为N，初始值为-1
        queue<int> q;
        dis[p[i]] = 0;
        q.push(p[i]);
	        
        while (!q.empty()) {
            int x = q.front();
            q.pop();
            
            if (dis[x] < d[i]) {
                black[x] = false;
            }
            for (auto y : adj[x]) {
                if (dis[y] == -1) {
                    dis[y] = dis[x] + 1;
	                    q.push(y);
                }
            }
        }
    }
    ***
        没搞懂为什么用bfs来搜，需要去做一下kuangbin搜索题
    似乎是因为要距离从小到大一层一层往外搜。
    
    bfs用于处理每条路径的距离，
    queue<int> q;
    vector<int> dis(N, -1);
    可以顺便初始化。
    for (int i = 0; i < N; i++) {
        if (black[i]) {
            q.push(i);
            dis[i] = 0;
        }
    }
    把可能的点推入。
    while (!q.empty()) {
        int x = q.front();
        q.pop();
        
        for (auto y : adj[x]) {
            if (dis[y] == -1) {
                dis[y] = dis[x] + 1;
                q.push(y);
            }
        }
    }
    处理出每个黑点的最短距离
    ***
    for (int i = 0; i < K; i++) {
        if (dis[p[i]] != d[i]) {
            cout << "No\n";
            return 0;
        }
    }
        没有最短距离为d[i]的点
        

    cout << "Yes\n";
    for (int i = 0; i < N; i++) {
        cout << black[i];
    }
    cout << "\n";
    
    return 0;
}
```

##### 模仿代码
```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;
int main(){
	int N,M;
	cin >> N >> M ;
	vector<vector<int> > adj(N);
	for(int i = 0;i < M;i++){
		int u,v;
		cin >> u >> v;
		u--,v--;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	vector<bool> black(N,1);
	int k;
	cin >> k;
	vector<int> p(k),d(k);
	for(int i = 0;i < k;i++){
		cin >> p[i] >> d[i];
		p[i] --;
		queue<int> q;
		vector<int> dis(N,-1);
		q.push(p[i]);
		dis[p[i]] = 0;
		while(q.size()){
			int x = q.front();
			q.pop();
			if(dis[x] < d[i]){
				black[x] = false;
			}
			for(auto y : adj[x]){
				if(dis[y] == -1){
					dis[y] = dis[x] + 1;
					q.push(y);
				}
			}
		}
	}
	queue<int> q;
	vector<int> dis(N,-1);
	for(int i = 0;i < N;i++){
		if(black[i]){
			q.push(i);
			dis[i] = 0;
		}
	}
	while(q.size()){
		int x = q.front();
		q.pop();
		for(auto y : adj[x]){
			if(dis[y] == -1){
				dis[y] = dis[x] + 1;
				q.push(y);
			}
		}
	}
	for(int i = 0;i < k;i++){
		if(dis[p[i]] != d[i]){
			cout << "No" << endl;
			return 0;
		}
	}
	cout << "Yes" << endl;
	for(int i = 0;i < N;i++){
		cout << black[i] ;
	}
	cout << "\n" ;
	return 0;
}


```

## 套路：
### 1）bfs无边权（权值为1)最短路
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
#### 应对：
#### 1。求一个点到n个点的最短路：
把这个点当作起点。
#### 2。 求1种点到距离另一种点的最短距离
把其中一种点全部推入循环，然后dis设置为0.n个起点。


### 关于为什么不用spfa，dijkstra
**小贴士：**  
	很多同学一看到「最短路径」，就条件反射地想到「Dijkstra 算法」。为什么 BFS 遍历也能找到最短路径呢？  
	这是因为，Dijkstra 算法解决的是**带权最短路径问题**，而我们这里关注的是**无权最短路径问题**。也可以看成每条边的权重都是 1。这样的最短路径问题，用 BFS 求解就行了。  
	在面试中，你可能更希望写 BFS 而不是 Dijkstra。毕竟，敢保证自己能写对 Dijkstra 算法的人不多。

**总结：**
bfs是处理无权边最短路的最优解。
spfa，dijkstra用来处理有权边
 有边权最短路 --> dijkstra，spfa
 无边权最短路 --> bfs
#bfs无边权最短路
