
**区間更新 + 区間の集約**が必要なときに使う。ACL の `lazy_segtree` ならどちらも `O(log N)`。

- 区間加算 + 区間和
- 区間加算 + 区間最大値 / 最小値
- 区間代入 + 区間和

## 実戦用：区間加算・区間和

```cpp
#include <atcoder/lazysegtree>
using namespace atcoder;
using ll = long long;

struct S {
    ll sum;
    int len;
};
using F = ll;

S op(S a, S b) {
    return {a.sum + b.sum, a.len + b.len};
}
S e() {
    return {0, 0};
}
S mapping(F f, S a) {
    return {a.sum + f * a.len, a.len};
}
F composition(F f, F g) {
    return f + g;
}
F id() {
    return 0;
}

// main 内
vector<ll> a(n, 0);
vector<S> v(n);
for (int i = 0; i < n; i++) v[i] = {a[i], 1};
lazy_segtree<S, op, e, F, mapping, composition, id> seg(v);

seg.apply(l, r, x);          // [l, r) の全要素に x を加算
ll sum = seg.prod(l, r).sum; // [l, r) の区間和
```

`S` に `len`（区間長）が必要：区間和が `sum` で、長さが `len` の区間に `x` を足すと、`sum += x * len` になる。

## 追加の5つの定義

通常の Segtree の `op`, `e` に加え、次を定義する。

| 記号 | 意味 |
|---|---|
| `S` | 区間に持たせる情報（和・長さなど） |
| `F` | 更新の種類と内容（加算値・代入値など） |
| `mapping(f, s)` | 区間の情報 `s` に更新 `f` を適用 |
| `composition(f, g)` | **先に g、そのあと f** を合成 |
| `id()` | 何もしない更新 |

区間加算なら `composition(f, g) = f + g`、`id() = 0`。

## 操作

```cpp
seg.apply(l, r, x); // [l, r) に更新 x を適用 : O(log N)
seg.apply(p, x);    // 位置 p に更新 x を適用 : O(log N)
seg.prod(l, r);     // [l, r) の区間情報 : O(log N)
seg.all_prod();     // 全体の情報 : O(1)
seg.set(p, value);  // 1点代入 : O(log N)
seg.get(p);         // 1点取得 : O(log N)
```

構築は `O(N)`。`lazy_segtree(n)` は全要素が `e()` になるので、区間長が必要な場合には**長さ1の `S` を詰めた vector で初期化**する。

## 変形：区間加算・区間最大値

**上の区間和用コードとは別の実装**。以下の定義に入れ替える。

```cpp
struct S {
    ll mx;
    int len;
};
using F = ll;

S op(S a, S b) {
    return {max(a.mx, b.mx), a.len + b.len};
}
S e() {
    return {LLONG_MIN, 0};
}
S mapping(F f, S a) {
    if (a.len == 0) return a; // 空区間は更新しない
    return {a.mx + f, a.len};
}
F composition(F f, F g) {
    return f + g;
}
F id() {
    return 0;
}

// main 内
vector<S> v(n);
for (int i = 0; i < n; i++) v[i] = {a[i], 1};
lazy_segtree<S, op, e, F, mapping, composition, id> seg(v);
seg.apply(l, r, x);
ll mx = seg.prod(l, r).mx;
```

最小値なら `mx` を `mn` と読み替え、`max` → `min`、`LLONG_MIN` → `LLONG_MAX` とする。

## 変形：区間代入・区間和

「加算」ではなく**代入**の場合、更新を上書きする。更新値0と「更新なし」を区別するため、`optional<ll>` を使う（C++17以降）。

```cpp
struct S {
    ll sum;
    int len;
};
using F = optional<ll>; // nullopt は更新なし

S op(S a, S b) {
    return {a.sum + b.sum, a.len + b.len};
}
S e() {
    return {0, 0};
}
S mapping(F f, S a) {
    if (!f.has_value()) return a;
    return {(*f) * a.len, a.len};
}
F composition(F f, F g) {
    return f.has_value() ? f : g; // 新しい代入 f が優先
}
F id() {
    return nullopt;
}

// main 内
vector<S> v(n);
for (int i = 0; i < n; i++) v[i] = {a[i], 1};
lazy_segtree<S, op, e, F, mapping, composition, id> seg(v);
seg.apply(l, r, F{x});
ll sum = seg.prod(l, r).sum;
```

## よくあるミス

- `[l, r)` の `r` は含まない。
- `mapping` で区間長を掛け忘れる（区間和の場合）。
- `composition(f, g)` の順序：**g を先に適用、f を後に適用**。
- `e()` と `id()` は別物（前者は集約の単位元、後者は更新の単位元）。
- 初期値が必要なら `vector<S>` にして、各葉の `len = 1` を設定。
- 数値の範囲が大きいと `sum + x * len` などがオーバーフローする。

## 関連

- [[Segtree（セグメント木）]]
- [[Fenwick Tree（BIT)]]
- [[imos法]]
- [ACL 公式ドキュメント](https://atcoder.github.io/ac-library/production/document_ja/lazysegtree.html)
