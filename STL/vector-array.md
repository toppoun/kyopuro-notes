
# vector 基本

```cpp
vector<int> a = {3, 1, 4};

a.push_back(2);
a.emplace_back(5);

int first = a.front();
int last = a.back();

a.pop_back();

int n = (int)a.size();
bool empty = a.empty();
```

`pop_back()` は値を返さない。

## 入力

```cpp
vector<int> a(n);
for (int& x : a) cin >> x;
```

## range-for

値を読むだけ。

```cpp
for (int x : a) {
    cout << x << ' ';
}
```

書き換える。

```cpp
for (int& x : a) {
    x *= 2;
}
```

コピーを避けたい構造体・コンテナでは `const auto&`。

```cpp
for (const auto& x : a) {
}
```

## 要素削除

位置を指定して1個削除。

```cpp
a.erase(a.begin() + i);
```

値 `x` をすべて削除。

```cpp
a.erase(remove(a.begin(), a.end(), x), a.end());
```

# array

要素数がコンパイル時に固定の配列。

```cpp
array<int, 3> a = {1, 2, 3};
```

普通に添字・range-for・`begin()` / `end()` が使える。

```cpp
for (int x : a) {
    cout << x << ' ';
}
```

固定サイズかつ要素数が小さいなら、`set` より `array` で全探索した方が単純で速いことが多い。

## 関連
- [[STL/sort-search]]
- [[典型/文字カウント]]
