
**1点更新 + 区間の集約**をそれぞれ `O(log N)` で行う。AtCoder Library（ACL）を使う。

- 区間和 / 区間最大値 / 区間最小値 / 区間 GCD など
- **全体の値**は `all_prod()` で `O(1)`
- **集約結果に基づく二分探索**は `max_right` / `min_left`
- 区間加算・区間代入が必要なら [[Lazy Segtree（遅延セグメント木）]]

## 実戦用：1点加算・区間最大値

```cpp
#include <atcoder/segtree>
using namespace atcoder;
using ll = long long;

ll op(ll a, ll b) { return max(a, b); }
ll e() { return LLONG_MIN; }

// main 内：初期値がすべて 0 の場合
vector<ll> a(n, 0);
segtree<ll, op, e> seg(a);

seg.set(p, seg.get(p) + x);  // a[p] += x
ll mx = seg.prod(l, r);      // [l, r) の最大値
ll all_mx = seg.all_prod();  // 全体の最大値
```

`seg.set(p, x)` は**加算でなく代入**。`seg.get(p)` は現在の1要素の値。

## op と e の意味

- `op(a, b)`：**左区間の集約値**と**右区間の集約値**を統合する。インデックスではない。
- `e()`：統合しても結果を変えない**単位元**。`op(a, e()) == op(e(), a) == a`。
- `op` は結合律が必要：`op(op(a,b),c) == op(a,op(b,c))`。引き算などはそのままでは使えない。

例えば `[2,1,3,4]` の区間和では、`op(2,1)=3`、`op(3,4)=7`、`op(3,7)=10` となる。

## 用途別の op / e

**使う組み合わせを1つだけ選ぶ**（以下を同時に定義しない）。

| 計算 | `op(a, b)` | `e()` |
|---|---|---|
| 区間和 | `a + b` | `0` |
| 区間最大値 | `max(a, b)` | `LLONG_MIN` |
| 区間最小値 | `min(a, b)` | `LLONG_MAX` |
| 区間GCD | `gcd(a, b)` | `0` |
| 区間積 | `a * b` | `1` |

型を `int` にするなら `INT_MIN` / `INT_MAX` を使う。整数オーバーフローにも注意。

## 操作一覧

| 操作 | 意味 | 計算量 |
|---|---|---|
| `seg.set(p, x)` | `a[p] = x` | `O(log N)` |
| `seg.get(p)` | `a[p]` | `O(1)` |
| `seg.prod(l, r)` | `[l, r)` の集約値 | `O(log N)` |
| `seg.all_prod()` | `[0, n)` の集約値 | `O(1)` |
| `seg.max_right(l, f)` | 左端 `l` から条件を満たす右端を二分探索 | `O(log N)` |
| `seg.min_left(r, f)` | 右端 `r` から条件を満たす左端を二分探索 | `O(log N)` |

構築は `O(N)`。区間は**半開区間 `[l, r)`**。

### 初期化の落とし穴

```cpp
segtree<ll, op, e> seg(n);   // 全要素が e() で初期化される
```

最大値で `e() = LLONG_MIN` なら**全要素は0にならない**。0で始める場合は `vector<ll> a(n, 0); segtree<ll, op, e> seg(a);` とする。

## max_right：条件を満たす区間の右端

`seg.max_right(l, f)` は、`f(seg.prod(l, r))` が真である最大の `r` を探す（`f` が単調な場合）。`f(e()) == true` が必要。

### 実例：最大値が最初に出現する位置

区間最大値の segtree で、`target` を現在の全体最大値とする。

```cpp
ll target = seg.all_prod();
int idx = seg.max_right(0, [&](ll v) {
    return v < target;
});
// idx = 左から最初の「最大値の要素」の0-indexed位置
```

`max` は右へ区間を広げても下がらないため、条件 `v < target` は単調に true → false と変わる。`target > e()` を満たす必要がある（非空の通常の得票数配列なら成立）。

### C - Second Choice Election で使った考え方

- 最初に第一希望の得票数を数える。
- 候補 `i` が辞退したら、その得票を0にして、その人たちの第二希望に加算する。
- `all_prod()` で辞退後の最大得票数を得る。
- `max_right` で**その得票数を持つ最小インデックス**を求める（同票なら候補番号が小さい方）。
- 元の票数に戻して次の候補へ。

```cpp
// op = max, e = -1、初期配列は票数0以上の想定
seg.set(i, 0);
for (int j : mp[i]) seg.set(j, seg.get(j) + 1);

int target = seg.all_prod();
int idx = seg.max_right(0, [&](int v) { return v < target; });
cout << idx + 1 << '\n';

seg.set(i, cnt[i]);
for (int j : mp[i]) seg.set(j, seg.get(j) - 1);
```

元の提出コードで `e() = -1` としたのは**全得票が0以上**だから問題ない。汎用テンプレでは `INT_MIN` / `LLONG_MIN` が安全。

## min_left

`seg.min_left(r, f)` は、右端 `r` を固定し `f(seg.prod(l, r))` が真である最小の `l` を探索する（単調性と `f(e())` に注意）。

## 関連

- [[Fenwick Tree（BIT)]]：1点加算と区間和に特化
- [[Lazy Segtree（遅延セグメント木）]]：区間更新が必要
- [[二分探索（答えを探すタイプ）]]：判定の単調性
- [ACL 公式ドキュメント](https://atcoder.github.io/ac-library/production/document_ja/segtree.html)
