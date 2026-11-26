
深さ優先探索。連結成分、到達可能性、グリッド探索、木の探索などで使う。

## グラフ

```cpp
vector<vector<int>> g(n);
vector<bool> seen(n);

auto dfs = [&](auto&& self, int v) -> void {
    seen[v] = true;

    for (int nv : g[v]) {
        if (seen[nv]) continue;
        self(self, nv);
    }
};

dfs(dfs, s);
```

## グリッド

4近傍。
例: `#` の連結成分を探索し、訪問済みを `.` に書き換える。

```cpp
int di[] = {-1, 1, 0, 0};
int dj[] = {0, 0, -1, 1};

auto dfs = [&](auto&& self, int i, int j) -> void {
    s[i][j] = '.';

    for (int d = 0; d < 4; d++) {
        int ni = i + di[d];
        int nj = j + dj[d];

        if (ni < 0 || ni >= h || nj < 0 || nj >= w) continue;
        if (s[ni][nj] == '.') continue;

        self(self, ni, nj);
    }
};
```

マスを書き換えてよいなら、`seen` を別に持たず `s[i][j] = '.'` のようにして訪問済みにできる。

## 連結成分の個数

```cpp
int cnt = 0;

for (int v = 0; v < n; v++) {
    if (seen[v]) continue;
    dfs(dfs, v);
    cnt++;
}
```

## BFSとの使い分け

- 到達可能性・連結成分・木の探索 → DFSでよいことが多い
- 重みなし最短距離 → BFS
- 木ではDFS/BFSどちらでも使える

頂点数が非常に大きく一直線につながる場合、再帰DFSはスタックが深くなりすぎることがある。その場合は `stack<int>` を使った反復DFSも候補。

## 関連
- [[アルゴリズム/BFS]]
- [[アルゴリズム/木の基本]]
