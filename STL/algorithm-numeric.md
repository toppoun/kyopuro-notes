# accumulate

合計。

```cpp
ll total = accumulate(a.begin(), a.end(), 0LL);
```

`ll` で欲しいなら初期値を `0LL` にする。

# iota

連番を作る。

```cpp
vector<int> p(n);
iota(p.begin(), p.end(), 0);
```

`0, 1, 2, ..., n-1` が入る。

# gcd / lcm

```cpp
int g = gcd(12, 18);
int l = lcm(12, 18);
```

# any_of

1つでも条件を満たすか。

```cpp
bool ok = any_of(a.begin(), a.end(), [](int x) {
    return x % 2 == 0;
});
```

# all_of

全部条件を満たすか。

```cpp
bool ok = all_of(a.begin(), a.end(), [](int x) {
    return x > 0;
});
```

# none_of

1つも条件を満たさないか。

```cpp
bool ok = none_of(a.begin(), a.end(), [](int x) {
    return x < 0;
});
```

# next_permutation

全順列。

最初に昇順にしておく。

```cpp
sort(p.begin(), p.end());

do {
    // p を使う
} while (next_permutation(p.begin(), p.end()));
```

要素数 N に対して N! 通りなので、小さい N だけ。

# 整数累乗

`pow()` は浮動小数点計算なので、整数累乗なら自前で書く方が安全。

指数が小さい場合。

```cpp
ll res = 1;
for (int i = 0; i < b; i++) {
    res *= a;
}
```

指数が大きい場合は二分累乗。

```cpp
ll res = 1;
while (b > 0) {
    if (b & 1) res *= a;
    a *= a;
    b >>= 1;
}
```

# 1-indexed の循環

`1, 2, ..., m, 1, 2, ...` と循環させる。

```cpp
(k - 1) % m + 1
```

0-indexed なら普通に `k % m`。

## 関連
- [[STL/sort-search]]
