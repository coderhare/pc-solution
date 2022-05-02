# 洛谷线段树模板题

分块维护的数据结构：

1. `st`表示第i号块的起点

2. `ed`表示第i号块的终点

3. `mark`表示第i号块整个区间打上的更新标记

4. `sum`表示第i号块离散加上的数据

5. `size`表示第i号块的大小

6. `bel`表示下标对应第i号块的位置

分块使得每次操作时间复杂度趋向于`O(sqrt(n))`，它的想法比较简单，但是码量却不小，对于某些题目，维护信息不满足交换律，可能使用分块优于线段树等

```c
void solve(){ 
    int n; cin >> n;
    vector<ll> a(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    int sq = sqrt(n);
    vector<ll> st(sq + 1), ed(sq + 1),  size(sq + 1), sum(sq + 1), mark(sq + 1);
    for (int i = 1; i <= sq; i++) {
        st[i] = n / sq * (i - 1) + 1;
        ed[i] = n / sq * i;
    }
    ed[sq] = n;
    for (int i = 1; i <= sq; i++) {
        size[i] = ed[i] - st[i];
    }
    vector<ll> bel(n + 1);
    for (int i = 1; i <= sq; i++) {
        for (int j = st[i]; j <= ed[i]; j++) {
            sum[i] += a[j];
            bel[j] = i;
        }
    }
    auto add = [&](int x, int y, int k){
        if(bel[x] == bel[y]){
            for (int i = x; i <= y; i++) {
                a[i] += k;
                sum[bel[i]] += k;
            }
        }
        else{
            for (int i = x; i <= ed[bel[x]]; i++) {
                a[i] += k;
                sum[bel[i]] += k;
            }
            for (int i = st[bel[y]]; i <= y; i++) {
                a[i] += k;
                sum[bel[i]] += k;
            }
            for (int i = bel[x] + 1; i < bel[y]; i++) {
                mark[i] += k;
            }
        }
    };
    auto query = [&](int x, int y)->ll{
        ll s = 0;
        if(bel[x] == bel[y]){
            for (int i = x; i <= y; i++) {
                s += a[i] + mark[bel[i]];
            }
        }
        else{
            for (int i = x; i <= ed[bel[x]]; i++) {
                s += a[i] + mark[bel[i]];
            }
            for (int i = st[bel[y]]; i <= y; i++) {
                s += a[i] + mark[bel[i]];
            }
            for (int i = bel[x] + 1; i < bel[y]; i++) {
                s += sum[i] + mark[i] * size[i];
            }
        }
        return s;
    };
    for (int i = 0; i < n; i++) {
        int op, l, r, k;
        cin >> op >> l >> r >> k;
        if(op == 0){
            add(l, r, k);
        }
        else{
            cout << query(l, r) << endl;
        }
    }
}

```
