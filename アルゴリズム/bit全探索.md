
N 個の要素について「使う / 使わない」の全 2^N 通りを試す。

```cpp
for (int mask = 0; mask < (1 << n); mask++) {
    for (int i = 0; i < n; i++) {
        if (mask & (1 << i)) {
            // i 番目を使う
        }
    }
}
```

計算量 O(N 2^N)。

`n` が大きめなら `1LL << n` を使う。

集合を表す mask として見ると、
- i bit目が1 → iを選ぶ
- i bit目が0 → iを選ばない

関連: [[典型/bit操作]]
