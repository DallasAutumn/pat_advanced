# 专题：Dijkstra+DFS解决多标尺最优路径问题

## 1. dijkstra求最短路
```cpp
vector<int> pre[MAXV];
void dijkstra(int s)
{
    fill(d, d + MAXV, INF);
    d[s] = 0;
    for(int i = 0; i < n; i++){
        int u = -1, MIN = INF;
        for(int j = 0; j < n; j++){
            if(!vis[j] && d[j] < MIN){
                u = j;
                MIN = d[j];
            }
        }
    }
    if(u == -1) return;
    vis[u] = true;
    for(int v = 0; v < n; v++){
        if(!vis[v] && G[u][v] != INF){
            if(d[u] + G[u][v] < d[v]){
                d[v] = d[u] + G[u][v];
                pre[v].clear();
                pre[v].push_back(u);
            }else if(d[u] + G[u][v] == d[v])
                pre[v].push_back(u);
        }
    }
}
```

## 2. dfs遍历pre递归树，搜索出使得第二标尺最优的路径（以边权和最小为例）
```cpp
int optvalue;
vector<int> pre[MAXV];
vector<int> path, tempPath;
void dfs(int v){
    if(v == st){
        tempPath.push_back(v);
        int value = 0;
        for(int i = tempPath.ize() - 1; i > 0; i--){
            int id = tempPath[i], idNext = tempPath[i - 1];
            value += G[id][idNext];
        if(value < optvalue){
            value = optvalue;
            path = tempPath;
        }
        tempPath.pop_back();
        return;
        }
    }
    tempPath.push_back(v);
    for(int i = 0; i < pre[v].size(); i++)
        dfs(pre[v][i]);
    tempPath.pop_back();
}
```