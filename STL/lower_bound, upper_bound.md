
ソート済み配列から、条件を満たす要素の境界を二分探索で求める。計算量は `O(log N)`。

## 基本

```cpp
int n = (int)a.size();
int left = (int)(lower_bound(a.begin(), a.end(), x) - a.begin());
int right = (int)(upper_bound(a.begin(), a.end(), x) - a.begin());
```

- `lower_bound`：`x` 以上の最初の位置
- `upper_bound`：`x` より大きい最初の位置


Pythonとの対応。

- `bisect_left(a, x)` → `lower_bound`
- `bisect_right(a, x)` → `upper_bound`


## インデックス取得

```cpp
// 条件を満たす最初の位置
int first_ge = left;   // x以上
int first_gt = right;  // xより大きい

// 条件を満たす最後の位置
int last_lt = left - 1;   // x未満
int last_le = right - 1;  // x以下
```

|条件|インデックス|
|---|---|
|`x` 以上の最初|`left`|
|`x` より大きい最初|`right`|
|`x` 未満の最後|`left - 1`|
|`x` 以下の最後|`right - 1`|
|`x` と等しい最初|`left`|
|`x` と等しい最後|`right - 1`|

`x` と等しい要素の位置は `left < right` の場合のみ有効。

## 個数計算

```cpp
int less_than_x = left;
int less_equal_x = right;
int greater_equal_x = n - left;
int greater_than_x = n - right;
int equal_x = right - left;
```

## 使用例

```cpp
vector<int> a = {1, 2, 2, 4, 6};
int x = 2;

int left = lower_bound(a.begin(), a.end(), x) - a.begin();  // 1
int right = upper_bound(a.begin(), a.end(), x) - a.begin(); // 3
```

- `x` 以上の最初：`a[1] = 2`
- `x` より大きい最初：`a[3] = 4`
- `x` 以下の最後：`a[2] = 2`
- `x` 未満の最後：`a[0] = 1`
- `x` と等しい個数：`3 - 1 = 2`


## 一部分だけ探索

```cpp
auto it = lower_bound(a.begin() + l, a.begin() + r, x);
int idx = (int)(it - a.begin());
```

半開区間 `[l, r)` の中から `x` 以上の最初の位置を探す。


## 降順配列

降順ソートした配列では `greater<>()` を指定する。

```cpp
sort(a.begin(), a.end(), greater<>());

int left = lower_bound(a.begin(), a.end(), x, greater<>()) - a.begin();
int right = upper_bound(a.begin(), a.end(), x, greater<>()) - a.begin();
```

- `left`：`x` 以下の最初の位置
- `right`：`x` 未満の最初の位置


## 注意点

- 基本的に昇順ソート済み配列で使う。
- `lower_bound` / `upper_bound` はイテレータを返す。
- `a.end()` が返る場合もあるため、参照前に確認する。
- `left == n`：`x` 以上の要素がない。
- `right == n`：`x` より大きい要素がない。
- `left == 0`：`x` 未満の要素がない。
- `right == 0`：`x` 以下の要素がない。
- `left == right`：`x` と等しい要素がない。
- `set` / `multiset` ではメンバ関数の `lower_bound()` / `upper_bound()` を使用する。

## 関連

- [[STL/sort-search]]
- [[二分探索（答えを探すタイプ）]]
- [[STL/set-map]]
- [[STL/multiset]]
- [[典型/座標圧縮]]