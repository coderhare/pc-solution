> 这个比赛有72个小时，我刚上去看的时候发现还剩下7个小时，就做了一下
> 感觉后面还有3道全是数学题，有点做不下去，希望有空能补上

### A
```c++
void solve() {
    int n;
    cin >> n;
    while(n--){
        int a, b;
        cin >> a >> b;
        cout << max(0, a - b) << endl;
    }
}

```

### B
```c++
void solve() {
    int n;
    cin >> n;
    while(n--){
        int a, b;
        cin >> a >> b;
        cout << ((a - 1)/6 + 1) * 1LL * b << endl;
    }
}
```

### C
+1, +2, +1, +2...这样每2次为一回合会漏掉一个，规律来看就是以2位首项的等差数列
```c++
void solve() {
    int n;
    cin >> n;
    while(n--){
        int a, b;
        cin >> a >> b;
        //1 3 4 6 7 9 10 12
        //2 5 8 11
        int rest = b - a;
        if(rest < 0 || (rest % 3 == 2)) cout << "NO" << endl;
        else cout << "YES" << endl;
    }
}
```

### D
模式匹配，主要是看某个串是否完全匹配模式串，如果不能则答案+1
```c++
void solve() {
    int n;
    cin >> n;
    string a, b;
    cin >> a >> b;
    vector<vector<int>> pos(256);
    for (int i = 0; i < b.size(); i++) pos[b[i] - 'a'].push_back(i);
    int ans = 0;
    for (int i = 0; i < 26; i++) {
        bool ok = 0;
        for (int j: pos[i]) {
            if (a[j] != b[j]) {
                ok = 1;
                break;
            }
        }
        ans += ok;
    }
    cout << ans << endl;
}

```

### E
构造题，事实上更好的构造方法可以将[1,..., N]两段分开，然后交错拼接
下面的做法主要是观察到最明显的性质：只有后面为`[1,N]`满足N-1的diff，
```c++
void solve() {
    bitset<100010> st;
    int n;
    cin >> n;
    vector <int> ans(n + 1);
    ans[n] = n, ans[n - 1] = 1;
    st[n] = st[1] = 1;
    for (int i = n - 2; i > 0; i--){
        int last = ans[i + 1], t;
        if(last + i > 1 && last + i < n && !st[last + i]) st[last + i] = 1, t = last + i;
        else if(last - i > 1 && last - i < n && !st[last - i]) st[last - i] = 1, t = last - i;
        ans[i] = t;
    }
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << " \n"[i == n];
    }
}
```
### F
一点点数学，先找到它们的差值，两者的gcd是有办法达到这个差值的，让次小值达到这个值。然后思考，此时这两个数实际上是倍数关系，能有方案
使得他们减去某个值，也达到某个该最大公约数的约数的情况。
用O(sqrt(n))的筛法即可得到约数分布情况
```c++
void solve() {
    int a, b;
    cin >> a >> b;
    int d = abs(a - b);
    int ans = 0;
    for (int i = 1; i <= sqrt(d); i++){
        if(d % i == 0) {
            ans++;
            if(d != i * i) ans++;
        }
    }
    cout << ans << endl;
}
```
