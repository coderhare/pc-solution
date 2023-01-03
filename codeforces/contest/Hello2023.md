## A

容易发现只要存在`RL`字符串即可满足要求，于是只需要判断是否存在，如果不存在的话将`LR`进行反转

```cpp
void solve() {
    int n;
    string s;
    cin >> n >> s;
    // 每盏灯左边或者右边有灯
    //LLLRLRR
    if (n == 1 || count(all(s), 'L') == 0 || count(all(s), 'R') == 0) {
        cout << -1 << "\n";
        return;
    }
    int idx = 0;
    for (int i = 0; i < n-1; i++) {
        if (s[i] == 'L' && s[i+1] == 'R') {
            idx = i+1;
            break;
        }
    }
    cout << idx << "\n";
}

```

## B

{**a1,a2**,a3...ak}这样的一个序列，容易发现必须满足减去a[i] + a[i+1]之后的和为0，当n=3的时候显然不满足；

当n>3的时候，通过消元发现，a[i] = a[i+2*k]，进而，对剩余的元素，进行正负的划分即可，然后可以给他们安一个值，`lcm(a,b)/a`，`lcm(a,b)/b`.

```cpp
void solve() {
    int n;
    cin >> n;
    if (n == 3) {
        cout << "NO\n";
    }
    else if (~n&1){
        cout << "YES\n";
        for (int i = 1; i <= n; i+=2) {
            cout << 1 << " " << -1 << " ";
        }
        cout << "\n";
    }
    else {
        cout << "YES\n";
        ll a = (n-1)/2, b = (n-2)/2;
        cout << lcm(a,b)/a << " ";
        for (int i = 1; i <= n-1; i+=2) {
            cout << -lcm(a,b)/b << " " << lcm(a,b)/a << " ";
        }
        cout << "\n";
    }
}
```

## C

对于m之前的所有前缀应该满足>=0，对于m之后的所有前缀也是如此。

于是可以采取从m位置向前保证前缀和< 0的操作，如果不满足的话将原先存在的堆里头最大值变成负数；

往后的操作同理。 实际上就是**反悔堆**的操作

```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector <ll> a(n+1);
    for (int i = 0; i < n; i++) {
        cin >> a[i+1];
    }
    ll cur = 0;
    int ans = 0;
    priority_queue<int> q;
    for (int i = m; i >= 2; i--) {
        cur += a[i];
        q.push(a[i]);
        if (cur > 0) {
            auto u = q.top(); q.pop();
            cur -= 2 * u;
            ans++;
        }
    }
    priority_queue <int,vector<int>,greater<>> q1;
    cur = 0;
    for (int i = m+1; i <= n; i++) {
        cur += a[i];
        q1.push(a[i]);
        if (cur < 0) {
            auto u = q1.top(); q1.pop();
            cur -= 2 * u;
            ans++;
        }
    }
    cout << ans << "\n";
}
```

