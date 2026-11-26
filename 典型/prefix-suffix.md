
「ある区間を除いた残り」などを高速に求めるため、左からの答えと右からの答えを前計算する。

## 区間削除後の最大値

`prefix[i]` = `a[0] ... a[i-1]` の最大値  
`suffix[i]` = `a[i] ... a[n-1]` の最大値

```cpp
vector<ll> prefix(n + 1, -INF);
vector<ll> suffix(n + 1, -INF);

for (int i = 0; i < n; i++) {
    prefix[i + 1] = max(prefix[i], a[i]);
}

for (int i = n - 1; i >= 0; i--) {
    suffix[i] = max(suffix[i + 1], a[i]);
}
```

半開区間 `[l, r)` を削除した後の最大値は

```cpp
ll ans = max(prefix[l], suffix[r]);
```

maxだけでなく、min・gcdなど「左右を独立にまとめて最後に結合できる」処理にも使える。
