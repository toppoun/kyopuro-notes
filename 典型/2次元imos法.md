
大量の長方形加算をまとめて処理する。

長方形 `[r1, r2) × [c1, c2)` に x を足す。

```cpp
diff[r1][c1] += x;
diff[r1][c2] -= x;
diff[r2][c1] -= x;
diff[r2][c2] += x;
```

全部の更新後、横・縦に累積する。

```cpp
vector<vector<ll>> diff(h + 1, vector<ll>(w + 1));

for (int i = 0; i <= h; i++) {
    for (int j = 0; j < w; j++) {
        diff[i][j + 1] += diff[i][j];
    }
}

for (int j = 0; j <= w; j++) {
    for (int i = 0; i < h; i++) {
        diff[i + 1][j] += diff[i][j];
    }
}
```

長方形更新 O(1)、復元 O(HW)。

1次元imosを縦横それぞれに行うイメージ。
