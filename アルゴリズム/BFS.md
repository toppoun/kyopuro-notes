
重みなしグラフで最短距離を求める基本。

## 最短距離

```cpp
vector<int> dist(n, -1);
queue<int> q;

dist[s] = 0;
q.push(s);

while (!q.empty()) {
    int v = q.front();
    q.pop();

    for (int nv : g[v]) {
        if (dist[nv] != -1) continue;
        dist[nv] = dist[v] + 1;
        q.push(nv);
    }
}
```

最初に訪問した時点で最短距離が確定するので、後から `min` を取る必要はない。

計算量 O(V + E)。

## 多点BFS
始点が複数あるなら、最初に全部 `dist = 0` でqueueへ入れる。

```cpp
for (int s : starts) {
    dist[s] = 0;
    q.push(s);
}
```
