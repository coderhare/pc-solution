# [Codeforces Round #842 (Div. 2)](https://codeforces.com/contest/1768)



## A

观察到 **i! + (i-1)! = (i+1)*(i-1)!**，所以能保证的最大值为`k-1`

```cpp
void solve() {
    ll k;
    cin >> k;
    //(i-1)!*(i+1)
    //(i^2-1)*(i-2)
    cout << k-1 << "\n";
}
 
```



## B

由于该操作会影响顺序，其实我们只需要考虑递增的最长子序列即可

```cpp
void solve() {
    int n, k;
    cin >> n >> k;
    vector <int> id(n+1);
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        id[x] = i;
    }
    int t = 2;
    while (t <= n && id[t] > id[t-1]) t++;
    cout << (n - t + k) / k << "\n"; //注意此处ceil需要写成(n-1+t+k)/k之类的形式来计算
}
```



## C

贪心，首先1的话由于特殊性，只能出现最多一次，其他数字可以出现最多两次。

然后，贪心地先填小的数字，因为先填大的会导致后面的选择变少

```cpp
void solve() {
    int n;
    cin >> n;
    vector <int> a(n+1), st(n+1);
    vector <vector<int>> ans(2, vector<int>(n+1)), has(2, vector <int> (n+1, 1));
    vector <int> pos(2, 1);
    priority_queue <tp,vector<tp>,greater<>> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i <= n; i++) {
        st[a[i]]++;
        if (st[a[i]] > 2 || st[1] >= 2) {
            cout << "NO\n";
            return;
        }
        ans[st[a[i]]-1][i] = a[i];
        has[st[a[i]]-1][a[i]] = 0;
        q.emplace(a[i], st[a[i]]-1, i);
    }
    // 贪心，每次都填最小的
    while (q.size()) {
        auto [val, p, id] = q.top(); q.pop();
        int & x = pos[!p];
        while (x <= n && !has[!p][x]) x++;
        if (x > val) {
            cout << "NO\n";
            return;
        }
        ans[!p][id] = x;
        x++;
    }
    cout << "YES\n";
    for (int i = 0; i < 2; i++) {
        for (int j = 1; j <= n; j++) {
            cout << ans[i][j] << " ";
        }
        cout << "\n";
    }
}
```

## D

需要观察出以下性质：

1. 满足条件的序列只有n-1个，是从升序排序的序列里头挑出来相邻的两个进行逆序操作
2. 将一个打乱的序列恢复成升序排序的操作数为 **n-cycles**，其中 **cycles**为置换环的数目
3. 从升序序列变成满足条件的序列，如果相邻点不在一个集合内部的话，则需要额外的操作一次达到逆置；反之如果在一个集合内部的话，则操作数减一

```cpp
void solve() {
    int n;
    cin >> n;
    vector <int> id(n + 1), st(n + 1);
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        id[x] = i;
    }
    int ans = n;
    for (int i = 1; i <= n; i++) {
        if (st[i]) continue;
        int j = i;
        while (!st[j]) {
            st[j] = i;
            j = id[j];
        }
        ans--;
    }
    for (int i = 2; i <= n; i++) {
        if (st[i] == st[i - 1]) {
            cout << ans - 1 << "\n";
            return;
        }
    }
    cout << ans + 1 << "\n";
}
```





## 小结

构造题做得不够，思维比较僵化