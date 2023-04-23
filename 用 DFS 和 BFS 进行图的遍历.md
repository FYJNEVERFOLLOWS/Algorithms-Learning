**PTA数据结构与算法题目集（中文）**

**7-6 列出连通集 (25 分)**

给定一个有N个顶点和E条边的无向图，请用DFS和BFS分别列出其所有的连通集。假设顶点从0到N−1编号。进行搜索时，假设我们总是从编号最小的顶点出发，按编号递增的顺序访问邻接点。

**输入格式:**
输入第1行给出2个整数N(0<N≤10)和E，分别是图的顶点数和边数。随后E行，每行给出一条边的两个端点。每行中的数字之间用1空格分隔。

**输出格式:**
按照"{ v​1​​ v​2​​ ... v​k​​ }"的格式，每行输出一个连通集。先输出DFS的结果，再输出BFS的结果。

**输入样例:**
```
8 6
0 7
0 1
2 0
4 1
2 4
3 5
```

**输出样例:**
```
{ 0 1 4 2 7 }
{ 3 5 }
{ 6 }
{ 0 1 2 7 4 }
{ 3 5 }
{ 6 }
```

```cpp
#include <iostream>
#include <queue>

using namespace std;
int n, matrix[101][101]; // 使用邻接矩阵存储图
bool visited[101] = {}; // 存储访问状态，默认都是0
void DFS(int v) {
    visited[v] = true;
    printf("%d ", v);
    for(int i = 0; i < n; i++) {
        if(matrix[v][i] != 0 && !visited[i]) 
            DFS(i);
    }
}
void DFSTrave() {
    for(int i = 0; i < n; i++) {
        if(!visited[i]) {
            printf("{ ");
            DFS(i); // 遍历i所在的连通集
            printf("}\n");
        }
    }
}
bool inq[101] = {}; // BFS必备，存储是否入过队
void BFS(int v) {
    queue<int> q;
    inq[v] = true; // 标记入队
    q.push(v);
    while(!q.empty()) {
        int top = q.front();
        q.pop();
        printf("%d ", top);
        for(int i = 0; i < n; i++) {
            if(matrix[top][i] != 0 && !inq[i]) { // 没被访问的一定没有入队
                inq[i] = true; // 入队意味着即将被访问
                q.push(i);
            }
        }        
    }
}
void BFSTrave() {
    for(int i = 0; i < n; i++) {
        if(!inq[i]) {
            printf("{ ");
            BFS(i); // 遍历i所在的连通集
            printf("}\n");
        }
    }
}
int main()
{
    int e;
    cin >> n >> e;
    int v, w;
    for(int i = 0; i < e; i++) {
        cin >> v >> w;
        matrix[v][w] = matrix[w][v] = 1;
    }    
    DFSTrave();
    BFSTrave();
    return 0;
}
```