
# sort

昇順。

```cpp
sort(a.begin(), a.end());
```

降順。

```cpp
sort(a.begin(), a.end(), greater<>());
```

# reverse

現在の並びを逆順にする。ソートではない。

```cpp
reverse(a.begin(), a.end());
```

降順ソートなら次のどちらでもよい。

```cpp
sort(a.begin(), a.end(), greater<>());
sort(a.rbegin(), a.rend());
```

# ラムダ式ソート

例: `second` の昇順。

```cpp
sort(a.begin(), a.end(), [](const auto& x, const auto& y) {
    return x.second < y.second;
});
```

例: `first` の降順、同じなら `second` の昇順。

```cpp
sort(a.begin(), a.end(), [](const auto& x, const auto& y) {
    if (x.first != y.first) return x.first > y.first;
    return x.second < y.second;
});
```

# count

```cpp
int cnt = (int)count(a.begin(), a.end(), x);
```

# STL find

```cpp
auto it = find(a.begin(), a.end(), x);

if (it != a.end()) {
    int index = (int)(it - a.begin());
}
```

`string::find()` とは別物。


# binary_search

存在するかだけ欲しいとき。

```cpp
bool exists = binary_search(a.begin(), a.end(), x);
```

# ソート済みvectorへ挿入

```cpp
a.insert(lower_bound(a.begin(), a.end(), x), x);
```

挿入位置探索は O(log N) だが、`vector` の挿入自体は O(N)。

# min_element / max_element

```cpp
int mn = *min_element(a.begin(), a.end());
int mx = *max_element(a.begin(), a.end());
```

添字が欲しい場合。

```cpp
int mn_i = (int)(min_element(a.begin(), a.end()) - a.begin());
int mx_i = (int)(max_element(a.begin(), a.end()) - a.begin());
```

両方まとめて。

```cpp
auto [mn_it, mx_it] = minmax_element(a.begin(), a.end());
```

# unique

ソートして重複削除。

```cpp
sort(a.begin(), a.end());
a.erase(unique(a.begin(), a.end()), a.end());
```

## 範囲を渡す理由

STLアルゴリズムはコンテナそのものではなく `[first, last)` の範囲に対して動く。

```cpp
sort(a.begin() + 2, a.begin() + 5);
```

これなら一部分だけ処理できる。

## 関連
- [[STL/vector-array]]
- [[STL/string]]
- [[STL/set-map]]
