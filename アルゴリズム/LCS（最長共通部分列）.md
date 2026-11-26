
2つの文字列 s, t の最長共通部分列の長さを求めるDP。

`dp[i][j]` = `s` の先頭 i 文字と `t` の先頭 j 文字のLCS長。

```cpp
int n = s.size();
int m = t.size();

vector<vector<int>> dp(n + 1, vector<int>(m + 1));

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        if (s[i] == t[j]) {
            dp[i + 1][j + 1] = dp[i][j] + 1;
        } else {
            dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j]);
        }
    }
}

cout << dp[n][m] << '\n';
```

計算量 O(NM)。
