# 拓扑排序与关键路径

## 1.拓扑排序
```cpp
stack<int> topOrder; // 拓扑排序过的顶点入栈，用来获得逆拓扑序列

bool topologicalSort()
{
    queue<int> q;

    for(int i = 0; i < n; i++)
    {
        // 入度为0的点进队
        if(inDegree[i] == 0)
            q.push(i);
    }

    while(!q.empty())
    {
        int u = q.front();
        q.pop();
        topOrder.push(u);

        for(edge edg : G[u])
        {
            int v = edg.v, w = edg.w;
            inDegree[v]--;
            if(inDegree[v] == 0)
                q.push(v);
        }

        // 更新后继节点的ve
        ve[v] = max(ve[v], ve[u] + w);
    }

    if(topOrder.size() == n)
        return true;
    else
        return false;
}
```

## 2.关键路径
```cpp
int criticalPath()
{
    fill(ve, ve + N, 0);
    if(!topologicalSort())
        return -1;
    fill(vl, vl + N, ve[n - 1]); //假设n-1号顶点是唯一汇点

    while(!topOrder.empty())
    {
        int u = topOrder.top();
        topOrder.pop();
        for(edge edg : G[u])
        {
            int v = edg.v, w = edg.w;
            // 更新前驱结点的vl
            vl[u] = min(vl[u], vl[v] - w);
        }
    }

    for(int u = 0; u < n ; u++)
    {
        for(edge edg : G[u])
        {
            int v = edg.v, w = edg.w;
            int e = ve[u], l = vl[v] - w;
            if(e == l)
                printf("%d->%d\n", u, v);
        }
    }

    return ve[n - 1];
}