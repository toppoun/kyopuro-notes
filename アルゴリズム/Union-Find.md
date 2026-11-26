
集合の結合と「同じグループか」の判定を高速に行う。

連結判定、グループ数、辺を順番に追加する問題などで使う。

## 実戦用

```cpp
struct UnionFind {
    vector<int> parent;
    vector<int> sz;

    UnionFind(int n) : parent(n), sz(n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }

    int root(int x) {
        if (parent[x] == x) return x;
        return parent[x] = root(parent[x]);
    }

    bool unite(int a, int b) {
        a = root(a);
        b = root(b);

        if (a == b) return false;

        if (sz[a] < sz[b]) swap(a, b);

        parent[b] = a;
        sz[a] += sz[b];

        return true;
    }

    bool same(int a, int b) {
        return root(a) == root(b);
    }

    int size(int x) {
        return sz[root(x)];
    }
};
```

## 使い方

```cpp
UnionFind uf(n);

uf.unite(a, b);

if (uf.same(a, b)) {
    // 同じ連結成分
}

cout << uf.size(a) << '\n';
```

## グループ数

最初は n 個のグループがあり、異なる集合を結合するたびに1減る。

```cpp
int groups = n;

if (uf.unite(a, b)) {
    groups--;
}
```

## 覚え方

- `root(x)` → x の代表
- `unite(a, b)` → 集合を結合
- `same(a, b)` → 同じ集合か
- `size(x)` → x が属する集合のサイズ

経路圧縮 + union by size により、1操作はほぼ O(1)。

## 関連
- [[アルゴリズム/DFS]]
