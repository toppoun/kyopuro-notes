
大量の区間加算をまとめて処理する。

半開区間 `[l, r)` に x を足すなら

```cpp
vector<ll> diff(n + 1);

diff[l] += x;
diff[r] -= x;
```

全部の更新後に累積する。

```cpp
for (int i = 0; i < n; i++) {
    diff[i + 1] += diff[i];
}
```

すると `diff[i]` が位置 i の最終値になる。

区間更新 O(1)、復元 O(N)。

## 覚え方
始点で足す、終点で戻す。
