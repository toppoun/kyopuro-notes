
**1点加算 + 区間和**に特化した軽量なデータ構造。Binary Indexed Tree（BIT）とも呼ぶ。

- `add(p, x)`：1要素に `x` を加算 → `O(log N)`
- `sum(l, r)`：区間 `[l, r)` の和 → `O(log N)`
- `segtree` でもできるが、足し算だけなら Fenwick Tree は定義が少なくて済む。

## 実戦用：1点加算・区間和

```cpp
#include <atcoder/fenwicktree>
using namespace atcoder;
using ll = long long;

// main 内。初期値はすべて0
fenwick_tree<ll> fw(n);

fw.add(p, x);              // a[p] += x
ll sum = fw.sum(l, r);     // [l, r) の区間和
ll total = fw.sum(0, n);   // 全体の和
ll value = fw.sum(p, p + 1); // a[p] の値
```

`fw.add(p, x)` は**加算**。`segtree` の `set(p, x)` は**代入**なので混同しない。

## 初期配列があるとき

```cpp
fenwick_tree<ll> fw(n);
for (int i = 0; i < n; i++) fw.add(i, a[i]);
```

初期化 `O(N)`、上の `add` を `N` 回行うと `O(N log N)`。

## 仕組み

部分和を飛び飛びに保持するため、すべての要素を毎回足さなくても範囲和が得られる。

`sum(l, r) = sum(0, r) - sum(0, l)` の考え方が基本。ACL の API は `sum(l, r)` であり、**`sum(r)` ではない**。

## 応用：区間加算・1点取得

差分配列を Fenwick Tree に持てば、オンラインで区間加算できる。`[l, r)` に `x` 加算する場合：

```cpp
fenwick_tree<ll> diff(n);

// 区間加算
// l, r, x をクエリごとに読み込む
// diff は差分配列を保持
// [l, r) への加算：
diff.add(l, x);
if (r < n) diff.add(r, -x);

// 位置 p の値（元の配列がすべて0の場合）
ll value = diff.sum(0, p + 1);
```

区間加算 `O(log N)`、1点取得 `O(log N)`。最終配列だけ欲しいなら [[imos法]] のほうが簡単で高速。

## 向いていないもの

- 区間最大値 / 区間最小値 → [[Segtree（セグメント木）]]
- 区間加算 + 区間最大値・区間和 → [[Lazy Segtree（遅延セグメント木）]]

## 関連

- [[Segtree（セグメント木）]]
- [[Lazy Segtree（遅延セグメント木）]]
- [[典型/累積和]]
- [[imos法]]
- [ACL 公式ドキュメント](https://atcoder.github.io/ac-library/production/document_ja/fenwicktree.html)
