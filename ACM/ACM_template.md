# 基本框架

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned int ull;
typedef pair<int, int> pil;
const ll ll_INF = 0x3f3f3f3f3f3f3f3f;
const ll ll_MAX = 0x7fffffffffffffff;
const int int_INF = 0x3f3f3f3f;
const int int_MAX = 0x7fffffff;
const double EPS = 1e-7;
const double PI = acos(-1.0);
const int dy[8] = {-1, 0, 1, 0, 1, -1, 1, -1};
const int dx[8] = {0, -1, 0, 1, 1, -1, -1, 1};
#define R register
#define _max(a, b) ((a) > (b) ? (a) : (b))
#define _min(a, b) ((a) < (b) ? (a) : (b))
#define _abs(a) ((a) > 0 ? (a) : -(a))
#define _swap(a, b) ((a) ^= (b) ^= (a) ^= (b))
#define _eql(x, y) (_abs((x) - (y)) < EPS)
const int MOD = 1e9 + 7;
const int MX = 1000010;
ll _r() {
    ll x = 0, f = 1;
    char c = getchar();
    while(c > '9' || c < '0') {
        if(c == '-')
            f = -1;
        c = getchar();
    }
    while(c >= '0' && c <= '9')
        x = (x << 3) + (x << 1) + (c ^ 48), c = getchar();
    return f == -1 ? -x : x;
}
void Init() {
}
void solve() {
}
int main() {
    std::ios::sync_with_stdio(false);
#ifdef LOCAL
    freopen("data.in", "r", stdin);
    freopen("data.out", "w", stdout);
#endif
    int t = 1;
    // cin>> t;
    while(t--) {
        Init();
        solve();
    }
    return 0;
}
```

## 常用命令

### 关闭流同步加速cin

```c++
std::ios::sync_with_stdio(false);
```

### 手动扩栈

```c++
#pragma comment(linker, "/STACK:10240000,10240000")
```



# STL

## 数组 / vector

### unique()

$O(n)$  去除相邻重复元素
```c++
n=unique(a,a+n)-a;//n为删除后元素个数
```
```c++
auto last=unique(v.begin(),v.end());//last为删除后的尾迭代器
v.erase(last, v.end());
```

### next_permutation() & prev_permutation()

 $O(n)$  返回值为bool：是否存在下一个排列 
```c++
while(next_permutation(v.begin(),v.end()){ ... }//遍历全排列
```

### upper_bound( ) & lower_bound()

  $ O(log\ n)$  二分第一个 $ \geq x $(lower) 或者   $ < x $ (upper)  的地址，在不存在对应值时都为右端点
```c++
int pre=a[upper_bound(a,a+n,x)-a];//启，止，值
```

## map / multi_map

基于红黑树，必须是 multi_map 才允许重复的值
```c++
map<int,string>::iterator prev=mp.upper_bound( 10 );//值
```

### operator []

**在访问不存在的时会新插入一个** 并设为默认值（比如说 int 默认 0, string 默认 “”）

### mp.upper_bound() & mp.lower_bound()

$O(log\ n) $  返回以 $ key $ 排序的 upper_bound 和 lower_bound，在不存在对应值时都为右端点

### mp.count()

返回满足的个数

### mp.find()

  $O(log\ n)$  返回满足 的迭代器位置，若不存在则为 mp.end()     *整体上看效率比 count() 更优*

## unorder_map

基于哈希表

## set / multiset


# 高精

## 无负数

```c++
const int BASE = 10000;
const int MX_LEN = 1010;
struct Bign {
    int num[MX_LEN];
    int len;
    Bign() {
        memset(num, 0, sizeof(num));
        len = 1;
    }
    Bign(const ll x) { *this = x; }
    Bign(const string x) { *this = x; }
    Bign(const Bign& x) {
        memset(num, 0, sizeof(num));
        len = x.len;
        for (int i = 0; i < len; i++)
            num[i] = x.num[i];
    }
    void clean() {
        while (num[len - 1] == 0 && len != 1)
            len--;
    }
    Bign operator = (const ll x) {
        stringstream ss;
        ss << x;
        string temp;
        ss >> temp;
        return *this = temp;
    }
    Bign operator = (const string x) {
        len = 0;
        memset(num, 0, sizeof(num));
        ll temp = 0;
        ll base = 1;
        for (int i = x.length() - 1; i >= 0; i--) {
            temp += (x[i] - '0') * base;
            base *= 10;
            if (base == BASE) {
                num[len++] = temp;
                temp = 0;
                base = 1;
            }
        }
        num[len++] = temp;
        clean();
        return *this;
    }
    Bign operator + (const Bign& b) const {
        Bign c;
        c.len = _max(len, b.len) + 1;
        for (int i = 0; i < c.len; i++) {
            c.num[i] += num[i] + b.num[i];
            c.num[i + 1] += c.num[i] / BASE;
            c.num[i] %= BASE;
        }
        c.clean();
        return c;
    }
    Bign operator - (const Bign& b) const {//a-b保证a>b
        Bign c;
        c.len = _max(len, b.len);
        for (int i = 0; i < c.len; ++i) {
            c.num[i] += num[i] - b.num[i];
            if (c.num[i] < 0) {
                c.num[i] += BASE;
                c.num[i + 1] -= 1;
            }
        }
        c.clean();
        return c;
    }
    Bign operator << (const int& num) const {
        Bign c = *this;
        c.len += 10;
        for (R int i = 0; i < c.len; ++i) {
            c.num[i] <<= num;
            if (i && c.num[i - 1] >= BASE)
                ++c.num[i], c.num[i - 1] -= BASE;
        }
        c.clean();
        return c;
    }
    Bign operator >> (const int& num) const {
        Bign c = *this;
        for (R int i = len - 1; i >= 0; --i) {
            if ((c.num[i] & 1) && i)
                c.num[i - 1] += BASE;
            c.num[i] >>= num;
        }
        c.clean();
        return c;
    }
    Bign operator * (const Bign& b) const {
        Bign c;
        c.len = len + b.len + 5;
        for (int i = 0; i < c.len; ++i) {
            for (int j = 0; j < b.len; ++j) {
                c.num[i + j] += num[i] * b.num[j];
                c.num[i + j + 1] += c.num[i + j] / BASE;
                c.num[i + j] %= BASE;
            }
        }
        c.clean();
        return c;
    }
    Bign operator / (const ll& b) const { //大数除以long long
        Bign c;
        c.len = len;
        ll rest = 0;
        for (int i = len - 1; i >= 0; --i) {
            rest = rest * BASE + num[i];
            c.num[i] = rest / b;
            rest %= b;
        }
        c.clean();
        return c;
    }
    Bign operator / (const Bign& b) const {
        Bign c, rest, now, _base;
        now = *this;
        rest = b;
        _base = 1;
        while (now >= rest) {
            rest = rest << 1;
            _base = _base << 1;
        }
        while (_base.len > 1 || _base.num[0]) {
            if (now >= rest) {
                now -= rest;
                c += _base;
            }
            rest = rest >> 1;
            _base = _base >> 1;
        }
        c.clean();
        return c;
    }
    Bign operator % (const ll& b) const {
        return (*this) - ((*this) / b) * b;
    }
    Bign operator % (const Bign& b) const {
        return (*this) - ((*this) / b) * b;
    }
    Bign operator += (const Bign& b) {
        return (*this) = (*this) + b;
    }
    Bign operator -= (const Bign& b) {
        return (*this) = (*this) - b;
    }
    Bign operator *= (const Bign& b) {
        return (*this) = (*this) * b;
    }
    Bign operator /= (const ll& b) {
        return (*this) = (*this) / b;
    }
    Bign operator /= (const Bign& b) {
        return (*this) = (*this) / b;
    }
    Bign operator %= (const ll& b) {
        return (*this) = (*this) % b;
    }
    Bign operator %= (const Bign& b) {
        return (*this) = (*this) % b;
    }
    bool operator < (const Bign& b) const {
        if (len == b.len) {
            for (int i = len - 1; i >= 0; --i) {
                if (num[i] != b.num[i])
                    return num[i] < b.num[i];
            }
            return 0;
        }
        return len < b.len;
    }
    bool operator > (const Bign& b) const {
        if (len == b.len) {
            for (int i = len - 1; i >= 0; --i) {
                if (num[i] != b.num[i])
                    return num[i] > b.num[i];
            }
            return 0;
        }
        return len > b.len;
    }
    bool operator == (const Bign& b) const {
        if (len == b.len) {
            for (int i = len - 1; i >= 0; --i) {
                if (num[i] != b.num[i])
                    return 0;
            }
            return 1;
        }
        return 0;
    }
    bool operator != (const Bign& b) const {
        return !((*this) == b);
    }
    bool operator <= (const Bign& b) const {
        return !((*this) > b);
    }
    bool operator >= (const Bign& b) const {
        return !((*this) < b);
    }
    friend ostream& operator << (ostream& out, const Bign& x) {
        out << x.num[x.len - 1];
        for (int i = x.len - 2; i >= 0; i--) {
            int t = BASE / 10;
            while (x.num[i] < t && t>1) {
                out << 0;
                t /= 10;
            }
            out << x.num[i];
        }
        return out;
    }
    friend istream& operator >> (istream& in, Bign& x) {
        string temp;
        in >> temp;
        x = temp;
        return in;
    }
};
```

## 有负数

```c++
const int BASE = 10000;
const int MX_LEN = 1010;
struct Bign {
    ll num[MX_LEN];
    int len;
    int neg;
    Bign() {
        memset(num, 0, sizeof(num));
        len = 1;
        neg = 0;
    }
    Bign(const ll x) { *this = x; }
    Bign(const string x) { *this = x; }
    Bign(const Bign& x) {
        memset(num, 0, sizeof(num));
        len = x.len;
        neg = x.neg;
        for (R int i = 0; i < len; ++i)
            num[i] = x.num[i];
    }
    void clean() {
        while (num[len - 1] == 0 && len != 1)
            --len;
        if (len == 1 && num[0] == 0)
            neg = 0;
    }
    Bign operator = (const ll& x) {
        stringstream ss;
        ss << x;
        string temp;
        ss >> temp;
        return *this = temp;
    }
    Bign operator = (const string& x) {
        len = 0;
        memset(num, 0, sizeof(num));
        if (x[0] == '-')
            neg = 1;
        else
            neg = 0;
        ll temp = 0;
        ll base = 1;
        for (R int i = x.length() - 1; i >= neg; --i) {
            temp += (x[i] - '0') * base;
            base *= 10;
            if (base == BASE) {
                num[len++] = temp;
                temp = 0;
                base = 1;
            }
        }
        num[len++] = temp;
        clean();
        return *this;
    }
    Bign operator + (const Bign& b) const {
        if (neg ^ b.neg) {
            if (neg)
                return b - (-*this);
            else
                return (*this) - (-b);
        }
        else {
            Bign c;
            if (neg)
                c.neg = 1;
            c.len = _max(len, b.len) + 1;
            for (R int i = 0; i < c.len; ++i) {
                c.num[i] += num[i] + b.num[i];
                c.num[i + 1] += c.num[i] / BASE;
                c.num[i] %= BASE;
            }
            c.clean();
            return c;
        }
    }
    Bign operator - (const Bign& b) const {
        if (neg ^ b.neg) {
            if (neg)
                return -((-*this) + b);
            else
                return (*this) + (-b);
        }
        else {
            Bign c;
            if (neg)
                c.neg = 1;
            if (abs(*this) < abs(b)) {
                c.neg ^= 1;
                c.len = _max(len, b.len);
                for (R int i = 0; i < c.len; ++i) {
                    c.num[i] += b.num[i] - num[i];
                    if (c.num[i] < 0) {
                        c.num[i] += BASE;
                        c.num[i + 1] -= 1;
                    }
                }
                c.clean();
                return c;
            }
            else {
                c.len = _max(len, b.len);
                for (R int i = 0; i < c.len; ++i) {
                    c.num[i] += num[i] - b.num[i];
                    if (c.num[i] < 0) {
                        c.num[i] += BASE;
                        c.num[i + 1] -= 1;
                    }
                }
                c.clean();
                return c;
            }

        }
    }
    Bign operator - () const {
        Bign c = *this;
        c.neg ^= 1;
        return c;
    }
    Bign operator * (const Bign& b) const {
        Bign c;
        c.len = len + b.len + 1;
        for (R int i = 0; i < len; ++i) {
            for (R int j = 0; j < b.len; ++j) {
                c.num[i + j] += num[i] * b.num[j];
            }
        }
        for (R int i = 0; i < c.len; ++i) {
            c.num[i + 1] += c.num[i] / BASE;
            c.num[i] %= BASE;
        }
        if (neg ^ b.neg)
            c.neg = 1;
        c.clean();
        return c;
    }
    Bign operator / (const ll& b) const {
        Bign c;
        c.len = len;
        ll rest = 0;
        for (R int i = len - 1; i >= 0; --i) {
            rest = rest * BASE + num[i];
            c.num[i] = rest / b;
            rest %= b;
        }
        if (neg ^ (b < 0))
            c.neg = 1;
        c.clean();
        return c;
    }
    Bign operator / (const Bign& b) const {
        Bign c, rest, now, _base;
        now = abs(*this);
        rest = abs(b);
        _base = 1;
        while (now >= rest) {
            rest <<= 1;
            _base <<= 1;
        }
        while (_base.len > 1 || _base.num[0]) {
            if (now >= rest) {
                now -= rest;
                c += _base;
            }
            rest >>= 1;
            _base >>= 1;
        }
        if (neg ^ b.neg)
            c.neg = 1;
        c.clean();
        return c;
    }
    Bign operator << (const int& num) const {
        Bign c = *this;
        c.len += 10;
        for (R int i = 0; i < c.len; ++i) {
            c.num[i] <<= num;
            if (i && c.num[i - 1] >= BASE)
                ++c.num[i], c.num[i - 1] -= BASE;
        }
        c.clean();
        return c;
    }
    Bign operator >> (const int& num) const {
        Bign c = *this;
        for (R int i = len - 1; i >= 0; --i) {
            if ((c.num[i] & 1) && i)
                c.num[i - 1] += BASE;
            c.num[i] >>= num;
        }
        c.clean();
        return c;
    }
    Bign operator % (const ll& b) const {
        return (*this) - ((*this) / b) * b;
    }
    Bign operator % (const Bign& b) const {
        return (*this) - ((*this) / b) * b;
    }
    Bign operator += (const Bign& b) {
        return (*this) = (*this) + b;
    }
    Bign operator -= (const Bign& b) {
        return (*this) = (*this) - b;
    }
    Bign operator *= (const Bign& b) {
        return (*this) = (*this) * b;
    }
    Bign operator /= (const ll& b) {
        return (*this) = (*this) / b;
    }
    Bign operator /= (const Bign& b) {
        return (*this) = (*this) / b;
    }
    Bign operator %= (const ll& b) {
        return (*this) = (*this) % b;
    }
    Bign operator %= (const Bign& b) {
        return (*this) = (*this) % b;
    }
    Bign operator <<= (const int& num) {
        return (*this) = (*this) << num;
    }
    Bign operator >>= (const int& num) {
        return (*this) = (*this) >> num;
    }
    bool operator < (const Bign& b) const {
        if (neg && !b.neg)
            return 1;
        if (!neg && b.neg)
            return 0;
        if (neg) {
            if (len == b.len) {
                for (R int i = len - 1; i >= 0; --i) {
                    if (num[i] != b.num[i])
                        return num[i] > b.num[i];
                }
                return 0;
            }
            return len > b.len;
        }
        else {
            if (len == b.len) {
                for (R int i = len - 1; i >= 0; --i) {
                    if (num[i] != b.num[i])
                        return num[i] < b.num[i];
                }
                return 0;
            }
            return len < b.len;
        }
    }
    bool operator > (const Bign& b) const {
        if (neg && !b.neg)
            return 0;
        if (!neg && b.neg)
            return 1;
        if (neg) {
            if (len == b.len) {
                for (R int i = len - 1; i >= 0; --i) {
                    if (num[i] != b.num[i])
                        return num[i] < b.num[i];
                }
                return 0;
            }
            return len < b.len;
        }
        else {
            if (len == b.len) {
                for (R int i = len - 1; i >= 0; --i) {
                    if (num[i] != b.num[i])
                        return num[i] > b.num[i];
                }
                return 0;
            }
            return len > b.len;
        }
    }
    bool operator == (const Bign& b) const {
        if (neg != b.neg)
            return 0;
        if (len == b.len) {
            for (R int i = len - 1; i >= 0; --i) {
                if (num[i] != b.num[i])
                    return 0;
            }
            return 1;
        }
        return 0;
    }
    bool operator != (const Bign& b) const {
        return !((*this) == b);
    }
    bool operator <= (const Bign& b) const {
        return !((*this) > b);
    }
    bool operator >= (const Bign& b) const {
        return !((*this) < b);
    }
    friend Bign abs(const Bign& x) {
        Bign c = x;
        c.neg = 0;
        return c;
    }
    friend ostream& operator << (ostream& out, const Bign& x) {
        if (x.neg)
            out << '-';
        out << x.num[x.len - 1];
        for (R int i = x.len - 2; i >= 0; --i) {
            int t = BASE / 10;
            while (x.num[i] < t && t>1) {
                out << 0;
                t /= 10;
            }
            out << x.num[i];
        }
        return out;
    }
    friend istream& operator >> (istream& in, Bign& x) {
        string temp;
        in >> temp;
        x = temp;
        return in;
    }
};
```

# 数据结构

## 并查集

### 路径压缩

```c++
int _fa[MX] = { 0 };
int _reset() {
    for(int i = 0; i <= n; i++)
        _fa[i] = i;
}
int _find(int x) {
	return x == _fa[x] ? x : _fa[x] = _find(_fa[x]);
}
void _union(int x, int y) {
	int xx = _find(x), yy = _find(y);
	if (xx == yy)
		return;
	if (xx > yy)
		_swap(xx, yy);
	_fa[yy] = xx;
}
```

### 按秩合并

以树的深度为节点的秩

```c++
int _fa[MX] = {0}, _rank[MX] = {0};
int _reset() {
    for(int i = 0; i <= n; i++) {
        rank[i] = 0;
        _fa[i] = i;
    }
}
int _find(int x) {
    return x == _fa[x] ? x : _find(_fa[x]);
}
void _union(int x, int y) {
    int xx = _find(x), yy = _find(y);
    if(xx == yy)
        return;
    if(_rank[xx] < _rank[yy])
        _swap(xx, yy);
    _fa[yy] = xx;
    if(_rank[xx] == _rank[yy])
        _rank[xx]++;
}
```

### 带权并查集

```c++
class WeightUnionSet {
public:
    int n, fa[MX], num[MX];
    ll dis[MX];
    void reset(int size = 0) {
        if(size != 0)
            n = size;
        for(int i = 0; i <= n; i++) {
            fa[i] = i;
            num[i] = 1;
            dis[i] = 0;
        }
    }
    int _find(int x) {
        if(x == fa[x])
            return x;
        int root = _find(fa[x]);
        dis[x] += dis[fa[x]];
        return fa[x] = root;
    }
    void _union(int u, int v, ll val) {  // u-v=d
        _find(u), _find(v);
        val += dis[u] - dis[v];
        u = _find(u), v = _find(v);
        if(u == v)
            return;
        if(num[u] < num[v])
            _swap(u, v), val = -val;
        num[u] += num[v];
        dis[v] = val;
        fa[v] = u;
    }
    ll diff(int u, int v) {  // u-v
        _find(u), _find(v);
        return dis[v] - dis[u];
    }
    bool is_same(int x, int y) {
        return _find(x) == _find(y);
    }
    bool is_root(int x) {
        return x == _find(x);
    }
    int size(int x) {
        return num[_find(x)];
    }
};
```



## 单调队列
```c++
struct Node {
	int num;
	int id;
}a[MX];
deque<Node> q;
void solve() {
	Node now;
	for (int i = 1; i <= n; i++) {
		if (q.empty()) {
			q.push_back(a[i]);
		}
		else {
			now = q.back();
			while (now.num > a[i].num) {
				q.pop_back();
				if (q.empty())break;
				now = q.back();
			}
			q.push_back(a[i]);
			now = q.front();
			if (i - now.id >= k)
				q.pop_front();
		}
		if (i >= k)
			cout << q.front().num << " ";//当前区间最大为队首
	}
}
```

## ST表


```c++
ll ST[MX][30] = { 0 };
int Log2[MX] = { 0 };
void creat_ST() {
	Log2[1] = 0, Log2[2] = 1;
	for (int i = 3; i <= n; i++)
		Log2[i] = Log2[i >> 1] + 1;
	for (int i = 1; i <= n; i++)
		ST[i][0] = a[i];
	for (int i = 1; i < 25; i++) {
		for (int j = 1; j <= n - (1 << (i - 1)) + 1; j++) {
			ST[j][i] = _max(ST[j][i - 1], ST[j + (1 << (i - 1))][i -1]);//RMQ
		}
	}
}
ll query_ST(int l, int r) {
	int k = Log2[r - l + 1];
	return gcd(ST[l][k], ST[r - (1 << k) + 1][k]);
}
```

## 树状数组

```c++
ll BIT[MX] = { 0 };
int low_bit(int x) {
	return x & (-x);
}
void BIT_add(int i, ll val) {
	while (i <= n) {
		BIT[i] += val;
		i += low_bit(i);
	}
}
ll BIT_sum(int i) {
	ll s = 0;
	while (i > 0) {
		s += BIT[i];
		i -= low_bit(i);
	}
	return s;
}
```

## 线段树

注意在求RMQ时**不要使用三目运算符**！

```c++
struct Tree {
	int l, r;
	ll val;
	ll add;
}tr[MX << 2];
ll a[MX] = { 0 };
void pushdown(int i) {
	if (tr[i].add == 0)
		return;
	if (tr[i].l == tr[i].r) {
		tr[i].add = 0;
		return;
	}
	tr[i << 1].val += tr[i].add * (tr[i << 1].r - tr[i << 1].l + 1);
	tr[i << 1].add += tr[i].add;
	tr[i << 1 | 1].val += tr[i].add * (tr[i << 1 | 1].r - tr[i << 1 | 1].l + 1);
	tr[i << 1 | 1].add += tr[i].add;
	tr[i].add = 0;
}
void pushup(int i) {
	tr[i].val = tr[i << 1].val + tr[i << 1 | 1].val;
}
void build(int i, int l, int r) {
	tr[i].l = l, tr[i].r = r, tr[i].val = tr[i].add = 0;
	if (l == r) {
		tr[i].val = a[l];
		return;
	}
    int mid = (l + r) >> 1;
	build(i << 1, l, mid);
	build(i << 1 | 1, mid + 1, r);
	pushup(i);
}
ll query(int i, int l, int r) {
	if (l > tr[i].r || r < tr[i].l)
		return 0;
	if (l <= tr[i].l && r >= tr[i].r)
		return tr[i].val;
	pushdown(i);
	return (query(i << 1, l, r) + query(i << 1 | 1, l, r));
}
void update(int i, int l, int r, ll val) {
	if (r<tr[i].l || l>tr[i].r)
		return;
	if (l <= tr[i].l && r >= tr[i].r) {
		tr[i].val += (tr[i].r - tr[i].l + 1) * val;
		tr[i].add += val;
		return;
	}
	pushdown(i);
	update(i << 1, l, r, val);
	update(i << 1 | 1, l, r, val);
	pushup(i);
}
```

## KDTree - 静态

本质上是记录了区间数据，以轴坐标的中位数为分界线，每层坐标轴交换的排序二叉树 [资料](https://www.luogu.com.cn/blog/van/qian-tan-pian-xu-wen-ti-yu-k-d-tree)

```c++
struct KDTree {
    int lson, rson;
    Point point;
    Point mn;  //左下角点
    Point mx;  //右上角点
} tr[MX << 2];
Point poi[MX];
int KDcnt = 0;
bool cmpx(Point a, Point b) {
    if(a.x == b.x)
        return a.y < b.y;
    return a.x < b.x;
}
bool cmpy(Point a, Point b) {
    if(a.y == b.y)
        return a.y < b.y;
    return a.y < b.y;
}
void pushup(int id) {
    if(tr[id].lson) {
        tr[id].mn.x = min(tr[id].mn.x, tr[tr[id].lson].mn.x);
        tr[id].mn.y = min(tr[id].mn.y, tr[tr[id].lson].mn.y);
        tr[id].mx.x = max(tr[id].mx.x, tr[tr[id].lson].mx.x);
        tr[id].mx.y = max(tr[id].mx.y, tr[tr[id].lson].mx.y);
    }
    if(tr[id].rson) {
        tr[id].mn.x = min(tr[id].mn.x, tr[tr[id].rson].mn.x);
        tr[id].mn.y = min(tr[id].mn.y, tr[tr[id].rson].mn.y);
        tr[id].mx.x = max(tr[id].mx.x, tr[tr[id].rson].mx.x);
        tr[id].mx.y = max(tr[id].mx.y, tr[tr[id].rson].mx.y);
    }
}
int build(int axis, int l, int r) {
    if(l > r)
        return 0;
    int mid = (l + r) >> 1;
    if(axis == 1)
        nth_element(poi + l, poi + mid, poi + r + 1, cmpx);
    else
        nth_element(poi + l, poi + mid, poi + r + 1, cmpy);
    int now = ++KDcnt;
    tr[now].mx = tr[now].mn = tr[now].point = poi[mid];
    tr[now].lson = build(axis ^ 1, l, mid - 1);
    tr[now].rson = build(axis ^ 1, mid + 1, r);
    pushup(now);
    return now;
}
```



## 主席树

```c++
struct Node {
    int l, r;
    int val;
} tr[MX << 5];
int node_cnt = 0, root[MX];
int build(int l, int r) {
    int i = ++node_cnt;
    tr[i].l = tr[i].r = 0, tr[i].val = 0;
    if(l == r) {
        tr[i].val = a[l];
        return i;
    }
    int mid = (l + r) >> 1;
    tr[i].l = build(l, mid);
    tr[i].r = build(mid + 1, r);
    return i;
}
int update(int pre, int l, int r, int x, int val) {
    int i = ++node_cnt;
    tr[i] = tr[pre];
    if(l == r) {
        tr[i].val = val;
        return i;
    }
    int mid = (l + r) >> 1;
    if(mid >= x)
        tr[i].l = update(tr[i].l, l, mid, x, val);
    else
        tr[i].r = update(tr[i].r, mid + 1, r, x, val);
    return i;
}
int query(int now, int l, int r, int k) {
    if(l == r)
        return tr[now].val;
    int mid = (l + r) >> 1;
    if(mid >= k)
        return query(tr[now].l, l, mid, k);
    else
        return query(tr[now].r, mid + 1, r, k);
}
```

### 区间第 $ k  $ 小值

离散化 + 单点插入更新主席树 +前缀和求区间数量

```c++
int n, m, a[MX], b[MX];
struct Node {
    int l, r;
    int cnt;
} tr[MX * 32];
int node_cnt = 0, root[MX];
int build(int l, int r) {
    int i = ++node_cnt;
    tr[i].l = tr[i].r = 0, tr[i].cnt = 1;
    if(l == r)
        return i;
    int mid = (l + r) >> 1;
    tr[i].l = build(l, mid);
    tr[i].r = build(mid + 1, r);
    return i;
}
int update(int pre, int l, int r, int val) {
    int i = ++node_cnt;
    tr[i] = tr[pre], tr[i].cnt = tr[pre].cnt + 1;
    if(l == r)
        return i;
    int mid = (l + r) >> 1;
    if(mid >= val)
        tr[i].l = update(tr[pre].l, l, mid, val);
    else
        tr[i].r = update(tr[pre].r, mid + 1, r, val);
    return i;
}
int query(int pre, int now, int l, int r, int k) {
    if(l == r)
        return l;
    int num = tr[tr[now].l].cnt - tr[tr[pre].l].cnt, mid = (l + r) >> 1;
    if(num >= k)
        return query(tr[pre].l, tr[now].l, l, mid, k);
    else
        return query(tr[pre].r, tr[now].r, mid + 1, r, k - num);
}
void Init() {
    n = _r(), m = _r();
    for(int i = 1; i <= n; i++) {
        a[i] = b[i] = _r();
    }
}
void solve() {
    sort(b + 1, b + n + 1);
    int size = unique(b + 1, b + n + 1) - (b + 1);
    root[0] = build(1, size);
    for(int i = 1; i <= n; i++) {
        int p = lower_bound(b + 1, b + 1 + size, a[i]) - b;
        root[i] = update(root[i - 1], 1, size, p);
    }
    while(m--) {
        int l = _r(), r = _r(), k = _r();
        printf("%d\n", b[query(root[l - 1], root[r], 1, size, k)]);
    }
}
```



# 数学

## 基本类

### 矩阵

```c++
const int MAT_SIZ = 55;  //矩阵阶数
struct Mat {
    int num[MAT_SIZ][MAT_SIZ];
    Mat() {
        clear();
    }
    void clear() {  // 清零
        for(R int i = 0; i < MAT_SIZ; i++)
            for(R int j = 0; j < MAT_SIZ; j++)
                num[i][j] = 0;
    }
    void unit() {  // 单位矩阵化
        clear();
        for(R int i = 0; i < MAT_SIZ; i++)
            num[i][i] = 1;
    }
    Mat operator+(const Mat& b) const {
        Mat c;
        for(R int i = 0; i < MAT_SIZ; i++) {
            for(R int j = 0; j < MAT_SIZ; j++) {
                c.num[i][j] = num[i][j] + b.num[i][j];
                c.num[i][j] %= MOD;
            }
        }
        return c;
    }
    Mat operator*(const Mat& b) const {
        Mat c;
        for(R int i = 0; i < MAT_SIZ; i++) {
            for(R int j = 0; j < MAT_SIZ; j++) {
                for(R int k = 0; k < MAT_SIZ; k++) {
                    c.num[i][j] += ((ll)num[i][k] * b.num[k][j]) % MOD;
                    c.num[i][j] %= MOD;
                }
            }
        }
        return c;
    }
    Mat operator+=(const Mat& b) {
        return *this = (*this) + b;
    }
    Mat operator*=(const Mat& b) {
        return *this = (*this) * b;
    }
    bool is_empty() {
        for(R int i = 0; i < MAT_SIZ; i++)
            for(R int j = 0; j < MAT_SIZ; j++)
                if(num[i][j] != 0)
                    return 0;
        return 1;
    }
    bool is_unit() {
        for(R int i = 0; i < MAT_SIZ; i++) {
            if(num[i][i] != 1)
                return 0;
            for(R int j = i + 1; j < MAT_SIZ; j++)
                if(num[i][j] != 0 || num[j][i != 0])
                    return 0;
        }
        return 1;
    }
    friend Mat reverse(const Mat& a) {  //转置
        Mat c;
        for(R int i = 0; i < MAT_SIZ; i++) {
            for(R int j = i; j < MAT_SIZ; j++) {
                c.num[i][j] = a.num[j][i];
            }
        }
        return c;
    }
    friend Mat pow(Mat base, ll x) {  //矩阵快速幂
        Mat res;
        res.unit();
        while(x) {
            if(x & 1)
                res = res * base;
            base = base * base;
            x >>= 1;
        }
        return res;
    }
};
```



## 快速幂

计算 $base^n$

```c++
ll _pow(ll base, ll n) {
	ll pow_res = 1;
	while (n) {
		if (n & 1)
			pow_res = pow_res * base % MOD;
		base = base * base % MOD;
		n >>= 1;
	}
	return pow_res;
}
```

### 龟速乘

用于 $ MOD\geq 10^9 $ 时,乘法可能爆出 ll 的问题,通常与快速幂搭配使用

```c++
ll _mul(ll x, ll y, ll MOD) {
	ll res = 0;
	while (y) {
		if (y & 1)
			res = (res + x) % MOD;
		x = (x + x) % MOD;
		y >>= 1;
	}
	return res;
}
```

### 快速乘

另一种用于处理爆出 ll 的方法

```c++
ll _mul(ll a, ll b) {
	a %= MOD, b %= MOD;
	return (a * b - (ll)(((long double)a * b + 0.5) / MOD) * MOD) % MOD;
}
```

### 矩阵快速幂

计算递推式 $f_n=a_1 f_{n-1} + a_2 f_{n-2} + \dots +a_k f_{n-k} + T* n + C$ 。手推前 $ k $ 项后使用矩阵快速幂求解。求解过程与转移矩阵构造如下:(图中所使用的矩阵都是 $ k+3$ 阶方阵,  $k+1$ 和 $k+2$ 行\列为求 $n$ 的部分,  $k+3$ 行为求 $ C $ 的部分,不需要的话可以对应减少阶数)
$$
\left[
\begin{matrix}
 f_n    & \cdots  \\
 f_{n-1}& \cdots  \\
 f_{n-2}& \cdots  \\
 \vdots & \cdots  \\
 f_1    & \cdots  \\
 n      & \cdots  \\
 1      & \cdots  \\
 C      & \cdots  \\
\end{matrix}
\right]
=
\left[
\begin{matrix}
 a_1    & a_2    & \cdots & a_{k-1}& a_{k}  & 1      & 1      & 1         \\
 1      & 0      & \cdots & 0      & 0      & 0      & 0      & 0         \\
 0      & 1      & \cdots & 0      & 0      & 0      & 0      & 0         \\
 \vdots & \vdots & \ddots & \vdots & \vdots & \vdots & \vdots & \vdots \\
 0      & 0      & \cdots & 1      & 0      & 0      & 0      & 0         \\
 0      & 0      & \cdots & 0      & 0      & 1      & T      & 0         \\
 0      & 0      & \cdots & 0      & 0      & 0      & 1      & 0         \\
 0      & 0      & \cdots & 0      & 0      & 0      & 0      & 1         \\
\end{matrix}
\right]
^{n-k}
\left[
\begin{matrix}
 f_k    & \cdots  \\
 f_{k-1}& \cdots  \\
 f_{k-2}& \cdots  \\
 \vdots & \cdots  \\
 f_1    & \cdots  \\
 0      & \cdots  \\
 1      & \cdots  \\
 C      & \cdots  \\
\end{matrix}
\right]
$$

```c++
Mat mat_pow(Mat a, ll x) { //现已在MAT类里完成封装
	Mat res, base;
	res.unit(); base = a;
	while (x) {
		if (x & 1)
			res = res * base;
		base = base * base;
		x >>= 1;
	}
}
```
## 质因数

### 质数判断

```c++
bool Miller_Rabin(ll p) {
	if (p < 2) return 0;
	if (p != 2 && p % 2 == 0) return 0;
    //ll check_prime[6]= { 2 , 3 , 5 , 233 , 331 };
	ll check_prime[12]= {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37}; 
	ll s = p - 1;
	while (!(s & 1)) s >>= 1;
	for (int i = 0; i < 12; ++i) {
		if (p == check_prime[i]) return 1;
		ll t = s, m = _pow(check_prime[i], s, p);
		while (t != p - 1 && m != 1 && m != p - 1) {
			m = _mul(m, m, p);
			t <<= 1;
		}
		if (m != p - 1 && !(t & 1)) return 0;
	}
	return 1;
}
```

### 质数筛法

```c++
int pri[MX] = { 0 }, not_pri[MX] = { 0 };
void EularSieve(int range) {
	for (int i = 2; i < range; i++) {
		if (not_pri[i] == 0)
			pri[++pri[0]] = i;
		for (int j = 1; j <= pri[0]; j++) {
			if (pri[j] * i > range)break;
			not_pri[pri[j] * i] = 1;
			if (i % pri[j] == 0)break;
		}
	}
}
```

### 质因数分解

p[i]:质因数表 , pcnt[i]:对应质因数的次数

```c++
int p[MX] = { 0 }, pcnt[MX] = { 0 };
void PrimeDevide(ll num) {
	for (int i = 1; i < pri[0] && (ll)pri[i] * pri[i] <= num; i++) {
		if (num % pri[i] == 0) {
			p[++p[0]] = pri[i];
			while (num % pri[i] == 0) {
				pcnt[p[0]]++;
				num /= pri[i];
			}
		}
	}
	if (num != 1) {
		p[++p[0]] = num;
		pcnt[p[0]] = 1;
	}
}
```

### 快速质因数分解

再说Pollard-Rho算法

### 欧拉函数

求小于等于 $n$ 的正整数中与 $n$ 互质的数的数量

```c++
ll phi(ll x) {
    ll ans = x;
    for(ll i = 2; i * i < x; i++) {
        if(x % i == 0) {
            ans -= ans / i;
            while(x % i == 0)
                x /= i;
        }
    }
    if(x != 1)
        ans -= ans / x;
    return ans;
}
```



## $gcd$ 与 $lcm$

```c++
ll gcd(ll a, ll b) {
	return b == 0 ? a : gcd(b, a % b);
}
ll lcm(ll a, ll b) {
	return a / gcd(a, b) * b;
}
```

### 拓展欧几里得

求解 $ ax+by=\gcd (a,b)$ 的一组可行解并返回 $\gcd (a,b)$ 

```c++
ll exgcd(ll a, ll b, ll& x, ll& y) {
	if (b == 0) {
		x = 1; y = 0;
		return a;
	}
	else {
		ll tx, ty;
		ll d = exgcd(b, a % b, tx, ty);
		x = ty;
		y = tx - (a / b) * ty;
		return d;
	}
}
```

## 逆元

### 拓欧求法

```c++
ll get_inv(ll a) {
	ll x, y, d;
	d = exgcd(a, MOD, x, y);
	return d == 1 ? (x % MOD + MOD) % MOD : -1;
}
```

### 快速幂求法

$MOD$ 需要为**质数**

```c++
ll get_inv(ll a) {
	return pow(a, MOD - 2, MOD);
}
```

### 递推求法

#### $1 \sim n $ 所有数的逆元

```c++
int inv[MX] = { 0 };
void get_inv(int range) {
	inv[1] = 1;
	for (int i = 2; i <= range; i++)
		inv[i] = MOD - ((MOD / i) * inv[MOD % i]) % MOD;
}
```

#### 任意 $n$ 个数的逆元

a[i]:原数组下标从 1 开始,    s[i]:前缀积,    s_inv[i]:前缀积的逆元

```c++
int inv[MX] = { 0 }, s_inv[MX] = { 0 };
ll s[MX] = { 0 };
void get_set_inv(int range) {
	s[0] = 1;
	for (int i = 1; i <= range; i++)
		s[i] = s[i - 1] * a[i] % MOD;
	s_inv[n] = _pow(s[n], MOD - 2, MOD);
	for (int i = range; i >= 1; i--)
		s_inv[i - 1] = s_inv[i] * a[i] % MOD;
	for (int i = 1; i <= range; i++)
		inv[i] = s_inv[i] * s[i - 1] % MOD;
}
```

### 阶乘逆元

也可以顺势计算普通逆元

```c++
void get_fac_inv(int range) {
	fac[0] = 1;
	for (int i = 1; i <= range; i++)
		fac[i] = (fac[i - 1] * i) % MOD;
	fac_inv[range] = _pow(fac[range], MOD - 2, MOD);
	for (int i = range - 1; i >= 0; i--)
		fac_inv[i] = (fac_inv[i + 1] * (i + 1)) % MOD;
}
```

## 阶乘

### 求阶乘位数

斯特林公式： ~~精度感人~~
$$
n! \approx \sqrt{2 \pi n}*(\frac{n}{e})^n \ \Longleftrightarrow \  \ln n! \approx \frac{1}{2}\ln2\pi n +n\ln \frac{n}{e}
$$

```c++
double lg_fac(ll n) {
    return 0.5 * log10(2 * PI * n) + n * (log10(n) - log10(exp(1)));
}
ll get_fac_len(ll n) {
    if(n == 1)
        return 1;
    else
        return lg_fac(n) + 1;
}
```

## 组合数学

计算 $ A_n^m $ 和 $ C_n^m $ ( $ n $ 选 $m$ )，即 $n \choose m $，

### 逆元求法

```c++
ll _A(int n, int m) {
	if (m > n || m < 0)
		return 0;
	return (ll)fac[n] * fac_inv[n - m] % MOD;
}
ll _C(int n, int m) {
	if (m > n || m < 0)
		return 0;
	return (ll)fac[n] * fac_inv[n - m] % MOD * fac_inv[m] % MOD;
}
```

### Lucas定理

对较小质数$ p $ 取模，要求 $p \leq 10^6 $
$$
{n \choose m}={n\mod p \choose m\mod p} \cross {n/p \choose m/p}
$$

```c++
ll lucas(int n,int m){
    if(m>n)
        return 0;
    if(n<MOD)
        return _C(n, m);
    else
        return lucas(n / MOD, m / MOD) * lucas(n % MOD, m % MOD) % MOD;
}
```

### 计算性质

#### Pascal公式

$$
{n \choose k}={n-1 \choose k}+{n-1 \choose k-1}
$$

#### 置换式

$$
k {n \choose k}=n{n-1\choose k-1}
$$

#### 范德蒙德卷积公式

$$
\sum_{k=0}^m{m_1 \choose k}{m_2 \choose n-k}={m_1+m_2 \choose n}
$$

## 常用数列

### 斐波那契数列

$\{0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,1597,2584,4181,6765,10946,17711,28657,46368,75025,121393,196418,317811\}$

#### 求法

通项： $ \displaystyle F_n=\frac{1}{\sqrt5}*[(\frac{1+\sqrt5}{2})^n-(\frac{1-\sqrt5}{2})^n]$

递推： $ \displaystyle F_n= F_{n-1}+F_{n-2}$    (矩阵快速幂)

#### 循环节





#### 应用

### 卡特兰数

$\{1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845\}$

#### 求法

通项：$ \displaystyle C_n=\frac{1}{n+1} {2n \choose n}$	

递推：$\displaystyle C_n=\frac{4n+2}{n+2}C_{n-1}$	

极限：$\displaystyle C_n \sim \frac{4^n}{(\sqrt n)^3 \sqrt\pi}$

#### 应用

二叉树的方案数，栈的出栈顺序种数， $n$对括号的匹配方式

# 计算几何

## 基本类

### 二维向量

```c++
template<typename T>
struct Vector2 {
    T x, y;
    Vector2() {
        x = y = 0;
    }
    T get_len() {
        return sqrt(x * x + y * y);
    }
    Vector2 operator+(const Vector2& b) const {
        Vector2 c;
        c.x = x + b.x;
        c.y = x + b.y;
        return c;
    }
    Vector2 operator-(const Vector2& b) const {
        Vector2 c;
        c.x = x - b.x;
        c.y = x - b.y;
        return c;
    }
    Vector2 rotate(const double& theta) const {  //逆时针
        Vector2 c;
        c.x = x * cos(theta) - y * sin(theta);
        c.y = y * cos(theta) + x * sin(theta);
        return c;
    }
    friend T dot(const Vector2<T>& a, const Vector2<T>& b) {  //点乘
        return a.x * b.x + a.y * b.y;
    }
    friend T cross(const Vector2<T>& a, const Vector2<T>& b) {  //叉乘
        return a.x * b.y - a.y * b.x;
    }
};
typedef Vector2<int> Point;
typedef Vector2<double> Vec2;
```

## 面积

### 三角形面积

```c++
double get_S(Vec2 a, Vec2 b) {
	return _abs(cross(a, b)) / 2;
}
```

### 多边形面积

```c++
double get_S() {
	double res = cross(poi[0], poi[n - 1]);
	for (int i = 1; i < n; i++) {
		res += cross(poi[i], poi[i - 1]);
	}
	return _abs(res) / 2;
}
```

## 角度

### 旋转

计算 $\vec{v}=(x,y)$ 绕原点（起点）逆时针旋转 $\theta$ 后的向量

```c++
Vector2 rotate(Vector2& a, const double& theta) {
    Vector2 c;
    c.x = a.x * cos(theta) - a.y * sin(theta);
    c.y = a.y * cos(theta) + a.x * sin(theta);
    return c;
}
```

### 极角排序

逆时针排序向量或点，其中弧度比较常数较小，叉积比较精度较高

#### 极角

计算 $\vec{v}=(x,y)$ 顺时针到 $x$ 轴正半轴方向转过的角度 $\theta -\pi$，值域为 $(-\pi,\pi\ ]$ 

```c++
double theta = atan2(x, y);
```

#### 弧度比较法

```c++
bool cmp(const Point& p1, const Point& p2) {
    return atan2(p1.y, p1.x) < atan2(p2.y, p2.x);
}
```

#### 叉积比较法

```c++
int quad(const Point& p) {  //求象限
    return ((p.y < 0) ? 1 : 3) + ((p.x < 0) ^ (p.y < 0));
}
bool cmp(const Point& p1, const Point& p2) {
    if(quad(p1) != quad(p2))
        return quad(p1) < quad(p2);
    if(corss(p1, p2) != 0)
        return corss(p1, p2) > 0;
    return p1.x < p2.x;
}
```

## 距离

### 类型

#### 欧几里得距离 

$\sqrt{(x_1-x_2)^2+(y_1-y_2)}$

```c++
double e_dis(Point& a, Point& b) {
    return sqrt((ll)(a.x - b.x) * (a.x - b.x) + (ll)(a.y - b.y) * (a.y - b.y));
}
```

#### 曼哈顿距离

$\abs{x_1-x_2}+\abs{y_1-y_2}$

```c++
int m_dis(Point& a, Point& b) {
    return _abs(a.x - b.x) + _abs(a.y - b.y);
}
```

#### 切比雪夫距离

$max(\abs{x_1-x_2},\abs{y_1-y_2})$

```c++
int c_dis(Point& a, Point& b) {
    return _max(_abs(a.x - b.x), _abs(a.y - b.y));
}
```

#### 曼哈顿与切比雪夫转化

可以用于**求曼哈顿距离小于一定范围的点对**，**求切比雪夫距离之和**

通用一点的话就是：求曼哈顿距离之差(消去绝对值)，求切比雪夫距离之和（消掉max)，需要转化

主要思想是坐标变换，切比雪夫坐标系的等高线是曼哈顿坐标系的等高线的外接正方形：

已知点集 $(x,y)$，求任意点对的**曼哈顿距离**: 可将所有点变化为 $(x+y,x-y)$后求新坐标系下对应点对的**切比雪夫距离**；

已知点集 $(x,y)$，求任意点对的**切比雪夫距离**: 可将所有点变化为 $\displaystyle (\frac{x+y}{2},\frac{x-y}{2})$后求新坐标系下对应点对的**曼哈顿距离**；



# 字符串

## hash

### 乘法哈希

```c++
ull hash(string x) {
	ull res = 0;
	int hash_base = 33;
	//int hash_mod=402653189;
	for (int i = 0; i < x.length(); i++) {
		res = res * base + x[i];
		//res%=hash_mod;
	}
	return hash;
}
```

### 位运算哈希

```c++
ull hash(string x) {
	ull res = 0;
	for (int i = 0; i < x.length(); i++) {
		res = (res << 4) ^ (res >> 28) ^ x[i];
	}
	return res;
}
```

### FNV哈希

$$
& int: & res=  2166136261           &, &FNV\_prime=  16777619      &;\\
& ull: & res=  14695981039346656037 &, &FNV\_prime=  1099511628211 &;\\
$$

```c++
ull hash(string x) {
	ull res = 2166136261;
	int FNV_prime = 16777619;
	for (int i = 0; i < x.length(); i++) {
		hash ^= x[i];
		hash *= FNV_prime;
	}
	return hash;
}
```

## 字典树

```c++
struct Node {
	bool end;
	int son[26]; 
	Node() {
		end = 0;
		for (int i = 0; i < 26; i++)
			son[i] = 0;
	}
}tr[50010];
int tr_cnt = 0;
void insert(string& x) {
	int u = 0, v, lenx = x.length();
	for (int i = 0; i < lenx; i++) {
		v = x[i] - 'a';
		if (!tr[u].son[v])
			tr[u].son[v] = ++tr_cnt;
		u = tr[u].son[v];
	}
	tr[u].end = 1;
}
bool check(string& x) {
	int u = 0, v;
	for (int i = 0; i < x.length(); i++) {
		v = x[i] - 'a';
		if (!tr[u].son[v])
			return 0;
		u = tr[u].son[v];
	}
	return tr[u].end;
}
```

## KMP

```c++
int KMP_next[MX] = { 0 };
void get_next(string& b) {
	ll l = -1, r = 0;
	KMP_next[0] = -1;
	while (r < b.length()) {
		if (l == -1 || b[l] == b[r])
			KMP_next[++r] = ++l;
		else
			l = KMP_next[l];
	}
}
void KMP(string& a, string& b) {
	ll l = 0, r = 0, cnt = 0;
	while (r < a.length()) {
		if (l == -1 || a[r] == b[l])
			l++, r++;
		else
			l = KMP_next[l];
		if (l == b.length()) {//found
			cnt++;
			l = KMP_next[l];
		}
	}
}
```

#### 回文长度

```c++
int palindrome_len(string& x) {
    get_next(x);
    int len = x.length();
    if (len % (len - KMP_next[len]) == 0)
        return len / (len - KMP_next[len]);
    else
        return 1;
}
```

## Manacher

最长回文子串长度

```c++
int rid[MX << 1];
int Manacher(string s) {
	int pos = 0, r = 0, len = s.length();
	string new_s = "##";
	for (int i = 0; i < len; i++)
		new_s += s[i], new_s += '#';
	len = new_s.length();
	for (int i = 0; i < len; i++) {
		if (i < r)
			rid[i] = _min(rid[(pos << 1) - i], rid[pos] + pos - i);
		else
			rid[i] = 1;
		while (i - rid[i] >= 1 && i + rid[i] < len && new_s[i - rid[i]] == new_s[i + rid[i]])
			++rid[i];
		if (i + rid[i] > r) {
			pos = i;
			r = i + rid[i];
		}
	}
	int max_len = 0;
	for (int i = 1; i <= len; ++i)
		max_len = _max(max_len, rid[i] - 1);
	return max_len;
}
```
# DP

## 背包问题

### $0 1$ 背包

$dp[j]=\max(dp[j],\ dp[j-w[i]]+v[i])$ , 其中 $dp[i]$ 表示背包容量为 $i$ 时能取到的最大价值

```c++
for(int i = 0; i < n; i++) {
    for(int j = W; j >= w[i]; j--) {
        dp[j] = _max(dp[j], dp[j - w[i]] + v[i]);
    }
}
```

### 完全背包 / 多重背包

$dp[j]=\max(dp[j],\ dp[j-w[i]*k]+v[i]*k)$ , 其中 $dp[j]$ 表示背包容量为 $j$ 时能取到的最大价值, $k$ 为一组物品 $i$ 的数量（通常使用倍增优化）

```c++
for(int i = 0; i < n; i++) {
    int have = num[i]; //物品i的总数量
    for(int k = 1; have; k <<= 1) {
        k = _min(have, k);
        for(int j = W; j >= c[i] * k; j--) {
            dp[j] = _max(dp[j], dp[j - w[i] * k] + v[i] * k);
        }
        have -= k;
    }
}
```

### 超重 $01$ 背包

$w_i$ 很大但 $\sum v_i$ 很小时可以用价值作为进行转移的变量，下面给出01背包对应做法:

$dp[j]=\min(dp[j],\ dp[j-v[i]]+w[i])$ , 其中 $dp[i]$ 表示当取得价值为 $i$ 时背包的最小容量

```c++
dp[0] = 0;
for(int i = 1; i < MX; i++)
    dp[i] = ll_INF;
for(int i = 0; i < n; i++) {
    for(int j = MX - 1; j >= v[i]; j--) {//mx为总价值
        dp[j] = _min(dp[j], dp[j - v[i]] + w[i]);
    }
}
for(int i = MX - 1; i >= 0; i--) {
    if(dp[i] <= W) {
        ans=i;
        break;
    }
}
```

### 超重完全背包

终极版完全背包：$W$， $w_i$ ，$num_i$ 很大但 $\sum v_i$ , $n$ 很小时的背包，可以先以性价比贪心，选取一定量的最高性价比后将 $W$ 降至一个可接受的大小再进行 DP。可接受的范围其实是需要保证在这个范围内每个物品都有可能被取到，也就是 $n*\sum v_i$  。具体的实现过程是倒过来的，现在全物品范围内跑一次完全背包，再利用贪心将背包的剩余部分填充满。

```c++
bool cmp(int a, int b) {
    return v[a] * w[b] > v[b] * w[a];
}
void huge_bag() {
    int lim = 50 * 50 * 50 + 1;
    for(int i = 1; i <= lim; i++)
        dp[i] = ll_INF;
    for(int i = 0; i < n; i++) { //小范围DP
        ll have = _min(num[i], 50);
        num[i] -= have;
        for(ll k = 1; have; k <<= 1) {
            k = _min(have, k);
            for(int j = lim - 1; j >= v[i] * k; j--) {
                dp[j] = _min(dp[j], dp[j - v[i] * k] + w[i] * k);
            }
            have -= k;
        }
    }
    for(int i = 1; i < n; i++) id[i] = i;
    sort(id, id + n, cmp);//性价比排序
    ll ans = 0;
    for(int j = 0; j <= lim; j++) {// 贪心
        ll rest = W - dp[j], now = j;
        if(rest < 0)
            continue;
        for(int i = 0; i < n; i++) {
            ll div = _min(num[id[i]], rest / w[id[i]]);
            rest -= div * w[id[i]];
            now += div * v[id[i]];
        }
        ans = _max(ans, now);
    }
    cout << ans << endl;
}
```

### 巨大背包

$n \leq 40$ 但是 $W$ 和  $\sum v$  都很大时，可以使用分治的思想将其分为两部分，将问题转换为在前半找到总质量为 $w_1$ 和总价值为 $v_1$ 的部分, 在后半找到总质量为 $w_2$ 和总价值为 $v_2$ 的部分，使在$w_1 +w_2 \leq W$  条件下满足 $v_1+v_2$ 最大。核心做法其实是二分。

```c++
pil pre[2000010];
void huge_bag() {
    int len = n >> 1, cnt = 1;
    for(int i = 0; i < (1 << len); i++) {
        ll sumv = 0, sumw = 0;
        for(int j = 0; j < len; j++) {
            if(i & (1 << j)) {
                sumv += v[j], sumw += w[j];
            }
        }
        pre[i] = make_pair(sumw, sumv);  //注意是(w,v)
    }
    sort(pre, pre + (1 << len));
    for(int i = 1; i < (1 << len); i++)
        if(pre[cnt - 1].second < pre[i].second)
            pre[cnt++] = pre[i];
    ll ans = 0;
    for(int i = 0; i < (1 << (n - len)); i++) {
        ll sumv = 0, sumw = 0;
        for(int j = 0; j < n - len; j++) {
            if(i & (1 << j)) {
                sumv += v[len + j], sumw += w[len + j];
            }
        }
        if(sumw <= W) {
            ll id = lower_bound(pre, pre + cnt, make_pair(W - sumw, ll_INF)) - pre;
            ans = _max(ans, sumv + pre[id - 1].second);
        }
    }
    cout << ans << endl;
}
```



### 常见优化

当费用相同时，只需保留价值最高的；当价值一定时，只需保留费用最低的；更一般的，对于两件物品 $i,j$ ，若$v_i \geq v_j$ 且  $w_i \leq w_j$ ，只需保留物品 $i$ 。

# 图论

## 基本框架

### 链式前向星

```c++
struct Node {
	ll val;
	int v;
	int _next;
}e[MX << 1];
int head[MX], cnt = 0;
void addedge(int u, int v, ll val) {
	e[++cnt].v = v;
	e[cnt].val = val;
	e[cnt]._next = head[u];
	head[u] = cnt;
}
```

### 链接矩阵

#### 应用

1. 求某一长度路径数量

   即求 **对应链接矩阵的 $k$ 次幂后，各数位上数字之和**

   

## 最短路

### Dijkstra + 堆优化

```c++
priority_queue<pil, vector<pil>, greater<pil> > q;
int vis[MX] = { 0 }; ll dis[MX] = { 0 };
void dijkstra(int s) {
	q.push(make_pair(0, s));
    for(int i=0;i<=n;i++)dis[i]=ll_INF;
	dis[s] = 0;
	int u;
	while (!q.empty()) {
		pil a = q.top(); q.pop();
		u = a.second;
		if (vis[u])continue;
		vis[u] = 1;
		for (int i = head[u]; i; i = e[i]._next) {
			if (dis[u] + e[i].val < dis[e[i].v]) {
				dis[e[i].v] = dis[u] + e[i].val;
				q.push(make_pair(dis[e[i].v], e[i].v));
			}
		}
	}
}
```

### SPFA

```c++
queue<int> q;
int inq[MX]; ll dis[MX] = { 0 };
void SPFA(int s) {
    while (!q.empty())q.pop();
	q.push(s);
    for(int i=0;i<=n;i++)dis[i] = ll_INF;
	dis[s] = 0;
	while (!q.empty()) {
		int u = q.front(); q.pop();
		inq[u] = 0;
		for (int i = head[u]; i; i = e[i]._next) {
			ll temp = dis[u] + e[i].val;
			int v = e[i].v;
			if (temp < dis[v]) {
				dis[v] = temp;
				if (!inq[v]) {
					q.push(v);
					inq[v] = 1;
				}
			}
		}
	}
}
```
#### SPFA + SLF

```c++
deque<int> q;
int inq[MX], len[MX]; ll dis[MX] = { 0 };
int spfa_SLF(int u) {
    while (!q.empty())q.pop_back();
	q.push_back(u);
    for(int i=0;i<=n;i++)dis[i] = ll_INF;
    len[u] = dis[u] = 0;
	while (!q.empty()) {
		u = q.front(); q.pop_front();
		inq[u] = 0;
		for (int i = head[u]; i; i = e[i]._next) {
			int v = e[i].v;
			if (dis[v] > dis[u] + e[i].val) {
				dis[v] = dis[u] + e[i].val;
				//判定负环（不保证出现时用 e.g.差分约束）
				len[v] = len[u] + 1;
				if (len[v] > n) {
					//flag = 1;
					return 1;
				}
				if (!inq[v]) {
					inq[v] = 1;
					if (q.empty() || dis[v] < dis[q.front()])
						q.push_front(v);
					else
						q.push_back(v);
				}
			}
		}
	}
	return 0;
}
```

#### DFS_SPFA

```c++
int inq[MX]; ll dis[MX] = { 0 };
int DFS_SPFA(int u, int fa) {
	inq[u] = 1;
	int v;
	for (int i = head[u]; i; i = e[i]._next) {
		v = e[i].v;
		if (v == fa)
			continue;
		if (dis[v] > dis[u] + e[i].val) {
			if (inq[v]) {
				return 1;
			}
			dis[v] = dis[u] + e[i].val;
			if (DFS_SPFA(v, u))
				return 1;
		}
	}
	inq[u] = 0;
	return 0;
}
```

#### 差分约束

$addedge(u,v,val)$ 在使用最短路(找最小上界)时表示 $v-u \leq val$  ，使用最长路(找最大下界)时表示 $v-u \geq val$ 。以最短路（找最小上界）为例,常用逻辑关系转换：( $u,v \geq 1$ ; $0$ 号节点为超级原点)

| **文字表达** |     **加边方式**     |  **文字表达**   |       **加边方式**       |
| :----------: | :------------------: | :-------------: | :----------------------: |
|   $ u>v $    |      $(u,v,-1)$      |   $ u>v +val$   |      $(u,v,-val-1)$      |
| $ u\geq v $  |      $(u,v,0)$       | $ u\geq v +val$ |       $(u,v,-val)$       |
|  $ u = v $   | $(u,v,0)$，$(v,u,0)$ |     $u=val$     | $(0,u,val)$，$(u,0,val)$ |

剩下的就是SPFA+SLF判定负环是否存在解并计算上下界

### Floyd

```c++
ll dis[MX][MX] = { 0 };
void floyd() {
	for (int k = 1; k <= n; k++) {
		for (int i = 1; i <= n; i++) {
			if (dis[i][k] ^ ll_INF) {
				for (int j = 1; j <= i; j++) {
					if (dis[k][j] ^ ll_INF) {
						dis[i][j] = _min(dis[i][j], dis[i][k] + dis[k][j]);
						dis[j][i] = dis[i][j];
					}
				}
			}
		}
	}
}
```

### K短路(A*)

```c++
struct Ori {
	int u, v, val;
}ori[100010];
struct Node {
	int v, val;
	int next;
}e[100010 << 1];
int head[MX], vis[MX], cnt = 0;
int n, m, S, T, K;
void addedge(int u, int v, int val) {
	e[++cnt].v = v, e[cnt].val = val;
	e[cnt].next = head[u];
	head[u] = cnt;
}
ll pred[MX] = { 0 };
priority_queue<pil, vector<pil>, greater<pil> > q;
void dijkstra() {
	for (int i = 1; i <= n; i++) {
		pred[i] = INF;
		vis[i] = 0;
	}
	pred[T] = 0;
	q.push(make_pair(0, T));
	while (!q.empty()) {
		pil now = q.top(); q.pop();
		int u = now.second;
		if (vis[u])
			continue;
		vis[u] = 1;
		for (int i = head[u]; i; i = e[i].next) {
			int v = e[i].v;
			if (pred[v] > pred[u] + e[i].val) {
				pred[v] = pred[u] + e[i].val;
				q.push(make_pair(pred[v], v));
			}
		}
	}
}
int cc;
ll ans;
struct Road {
	ll val;
	int u;
	bool operator < (const Road& a) const {
		return val + pred[u] > a.val + pred[a.u];
	}
};
priority_queue<Road> q2;
int AStar() {
	while (!q2.empty())q2.pop();
	Road now, nex;
	now.u = S;
	now.val = 0;
	q2.push(now);
	while (!q2.empty()) {
		now = q2.top(); q2.pop();
		if (vis[now.u] > K)
			continue;
		vis[now.u]++;
		if (now.u == T && vis[now.u] == K) {
			ans = now.val;
			return 1;
		}
		for (int i = head[now.u]; i; i = e[i].next) {
			int v = e[i].v;
			nex.u = v;
			nex.val = now.val + e[i].val;
			q2.push(nex);
		}
	}
	return 0;
}
void Init() {
	n = _r(), m = _r();
	for (int i = 0; i < m; i++) {
		ori[i].u = _r(), ori[i].v = _r(), ori[i].val = _r();
		addedge(ori[i].v, ori[i].u, ori[i].val);
	}
	S = _r(), T = _r(), K = _r(); //起点 终点 K短路
	if (S == T) K++;
}
void solve() {
	//反图
	dijkstra();
	//重建
	cnt = 0;
	for (int i = 1; i <= n; i++)
		vis[i] = head[i] = 0;
	for (int i = 0; i < m; i++)
		addedge(ori[i].u, ori[i].v, ori[i].val);
	if (AStar())
		printf("%d\n", ans);
	else
		printf("-1\n");
}
```


## 最小环

```c++
ll dis[MX][MX] = { 0 };
void floyd() {
	ll min_loop = ll_INF;
	for (int k = 1; k <= n; k++) {
		for (int i = 1; i < k; i++) {
			if (e[i][k] ^ ll_INF) {
				for (int j = i + 1; j < k; j++) {
					if (e[k][j] ^ ll_INF && dis[j][i] ^ ll_INF) {
						min_loop = _min(min_loop, e[i][k] + e[k][j] + dis[j][i]);
					}
				}
			}
		}
		for (int i = 1; i <= n; i++) {
			if (dis[i][k] ^ ll_INF) {
				for (int j = 1; j <= i; j++) {
					if (dis[k][j] ^ ll_INF) {
						dis[i][j] = _min(dis[i][j], dis[i][k] + dis[k][j]);
						dis[j][i] = dis[i][j];
					}
				}
			}
		}
	}
}
```

## 强连通分量 

### Tarjan

```c++
int dfn[MX], low[MX], col[MX], col_cnt, DFS_clock;
int _stack[MX], ins[MX];
void tarjan(int u) {
	_stack[++_stack[0]] = u;
	ins[u] = 1;
	dfn[u] = low[u] = ++DFS_clock;
	for (int i = head[u]; i; i = e[i]._next) {
		int v = e[i].v;
		if (!dfn[v]) {
			tarjan(v);
			low[u] = _min(low[u], low[v]);
		}
		else if (ins[v])
			low[u] = _min(low[u], dfn[v]);
	}
	if (low[u] == dfn[u]) {
		col_cnt++;
		do {
			col[_stack[_stack[0]]] = col_cnt;
			ins[_stack[_stack[0]]] = 0;
		} while (_stack[_stack[0]--] != u);
	}
}
```

### 割点与桥

注意都是**无向图**概念，在判定桥时**边计数器请从1开始**

```c++
int dfn[MX], low[MX], DFS_clock = 0;
int is_cut[MX], bridge[MX << 1];
void tarjan(int u, int fa, int root) {
	dfn[u] = low[u] = ++DFS_clock;
	int now_size = 0;
	for (int i = head[u]; i; i = e[i]._next) {
		int v = e[i].v;
		if (!dfn[v]) {
			tarjan(v, u, root);
			low[u] = _min(low[u], low[v]);
			if (low[v] >= dfn[u]) {//割点的判定
				now_size++;
				if (u != root || now_size > 1)
					is_cut[u] = 1;
			}
			if (low[v] > dfn[u])//桥的判定
				bridge[i] = bridge[i ^ 1] = 1;
		}
		else if (v != fa)
			low[u] = _min(low[u], dfn[v]);
	}
}
```

### 2-SAT

$addedge(u,v)$ 表示选取 $u$ 后必选取 $v$ ，常用逻辑关系转换：

|     文字表达     |                  加边方式                  |  文字表达  | 加边方式  |
| :--------------: | :----------------------------------------: | :--------: | :-------: |
|  取 $u\ \&\ v$   |                 $(u+n,u)$                  | 必不取 $u$ | $(u,u+n)$ |
| 取 $u\ \ |\ \ v$ |            $(u+n,v)$，$(v+n,n)$            |  必取 $u$  | $(u+n,u)$ |
|  取 $u\wedge v$  | $(u+n,v)$，$(v+n,n)$，$(u,v+n)$，$(v,n+n)$ |            |           |

```c++
int 2_SAT() {
	for (int i = 1; i <= (n << 1); i++) {
		if (dfn[i] == 0) {
			tarjan(i);
		}
	}
	for (int i = 1; i <= n; i++) {
		if (col[i] == col[i + n]) {
			return 0;//不存在
		}
	}
	for (int i = 1; i <= n; i++) {
		if (col[i] < col[i + n]) //拓扑逆序选用
			printf("1 ");
		else
			printf("0 ");
	}
	printf("\n");
    return 1;
}
```



## 生成树

### 最小生成树

无向图概念

```c++
bool cmp(Node a, Node b) {
	return a.val < b.val;
}
ll kruskal() {
	ll MST = 0;
	for (int i = 1; i <= n; i++)
		_fa[i] = i;
	sort(ori, ori + m, cmp);
	int u, v, ok = 1;
	for (int i = 0; i < m; i++) {
		u = ori[i].u; v = ori[i].v;
		if (_find(u) == _find(v))
			continue;
		_union(u, v);
		MST += ori[i].val;
		if (++ok == n)
			return MST;
	}
	return -1;
}
```

### 最小树形图

有向图的最小生成树

```c++
ll in_val[MX] = {0};
int in_u[MX] = {0}, loop_id[MX] = {0}, fa[MX] = {0}, root;//为给定时需要遍历root
ll zhuliu() {
    ll DMST = 0;
    int num = n;
    while(1) {
        for(int i = 1; i <= num; i++)
            in_val[i] = ll_INF;
        for(int i = 1; i <= m; i++) {
            if(e[i].u != e[i].v && in_val[e[i].v] > e[i].val) {
                in_val[e[i].v] = e[i].val;
                in_u[e[i].v] = e[i].u;
            }
        }
        for(int i = 1; i <= num; i++)
            if(i != root && in_val[i] == ll_INF)
                return -1;
        int loop_cnt = 0;
        for(int i = 1; i <= num; i++)
            fa[i] = loop_id[i] = 0;
        for(int i = 1; i <= num; i++) {
            if(i == root)
                continue;
            DMST += in_val[i];
            int now = i;
            while(fa[now] != i && loop_id[now] == 0 && now != root) {
                fa[now] = i;
                now = in_u[now];
            }
            if(loop_id[now] == 0 && now != root) {
                loop_id[now] = ++loop_cnt;
                do {
                    now = in_u[now];
                    loop_id[now] = loop_cnt;
                } while(fa[now] != i);
            }
        }
        if(loop_cnt == 0)
            break;
        for(int i = 1; i <= num; i++)
            if(loop_id[i] == 0)
                loop_id[i] = ++loop_cnt;
        for(int i = 1; i <= m; i++) {
            if(loop_id[e[i].u] != loop_id[e[i].v])
                e[i].val -= in_val[e[i].v];
            e[i].u = loop_id[e[i].u], e[i].v = loop_id[e[i].v];
        }
        root = loop_id[root];
        num = loop_cnt;
    }
    return DMST;
}
```



### 严格次小生成树

```c++
int n, m, cc = 0;
struct Node {
	int u, v, val;
	bool use;
}ori[MX];
struct Edge {
	int v, _next;
	ll val;
}e[MX];
int head[MX], cnt = 0;
void addedge(int u, int v, ll val) {
	e[++cnt].v = v;
	e[cnt].val = val;
	e[cnt]._next = head[u];
	head[u] = cnt;
}
int _fa[MX] = { 0 };
int _find(int x) {
	return x == _fa[x] ? x : _fa[x] = _find(_fa[x]);
}
void _union(int x, int y) {
	int xx = _find(x), yy = _find(y);
	if (xx == yy)
		return;
	if (xx > yy)
		_swap(xx, yy);
	_fa[yy] = xx;
}
void Init() {
	n = _r(), m = _r();
	for (int i = 0; i < m; i++) {
		ori[i].u = _r(); ori[i].v = _r(); ori[i].val = _r();
	}
}
bool cmp(Node a, Node b) {
	return a.val < b.val;
}
ll kruskal() {
	ll MST = 0;
	for (int i = 1; i <= n; i++)
		_fa[i] = i;
	sort(ori, ori + m, cmp);
	int u, v, ok = 1;
	for (int i = 0; i < m; i++) {
		u = ori[i].u; v = ori[i].v;
		if (_find(u) == _find(v))
			continue;
		_union(u, v);
		addedge(ori[i].u, ori[i].v, ori[i].val);
		addedge(ori[i].v, ori[i].u, ori[i].val);
		ori[i].use = 1;
		MST += ori[i].val;
		if (++ok == n)
			return MST;
	}
	return -1;
}
int anc[MX][21] = { 0 }, max_e[2][MX][21] = { 0 }, deep[MX] = { 0 };
void DFS(int u, int fa) {
	anc[u][0] = fa;
	for (int i = 1; i <= 20; i++) {
		anc[u][i] = anc[anc[u][i - 1]][i - 1];
		max_e[0][u][i] = max(max_e[0][u][i - 1], max_e[0][anc[u][i - 1]][i - 1]);
		max_e[1][u][i] = max(max_e[1][u][i - 1], max_e[1][anc[u][i - 1]][i - 1]);
		if (max_e[0][u][i - 1] != max_e[0][anc[u][i - 1]][i - 1]) {
			max_e[1][u][i] = max(max_e[1][u][i], min(max_e[0][u][i - 1], max_e[0][anc[u][i - 1]][i - 1]));
		}
	}
	for (int i = head[u]; i; i = e[i]._next) {
		if (e[i].v ^ fa) {
			deep[e[i].v] = deep[u] + 1;
			max_e[0][e[i].v][0] = e[i].val;
			DFS(e[i].v, u);
		}
	}
}
int lca(int x, int y) {
	if (deep[x] < deep[y])swap(x, y);
	for (int i = 20; i >= 0; i--) {
		if (deep[anc[x][i]] >= deep[y]) {
			x = anc[x][i];
		}
	}
	if (x == y)return x;
	for (int i = 20; i >= 0; i--) {
		if (anc[x][i] ^ anc[y][i]) {
			x = anc[x][i];
			y = anc[y][i];
		}
	}
	return anc[x][0];
}
ll change(int u, int v, ll try_val) {
	int l = lca(u, v);
	int loopmax = -1, loopless = -1;
	for (int i = 20; i >= 0; i--) {
		if (deep[anc[u][i]] >= deep[l]) {
			if (try_val != max_e[0][u][i]) {
				if (loopmax < max_e[0][u][i]) {
					loopless = loopmax;
					loopmax = max_e[0][u][i];
				}
			}
			if (max_e[1][u][i] && try_val != max_e[1][u][i])
				loopless = max(loopless, max_e[1][u][i]);
			u = anc[u][i];
		}
	}
	for (int i = 20; i >= 0; i--) {
		if (deep[anc[v][i]] >= deep[l]) {
			if (try_val != max_e[0][v][i]) {
				if (loopmax < max_e[0][v][i]) {
					loopless = loopmax;
					loopmax = max_e[0][v][i];
				}
			}
			if (max_e[1][v][i] && try_val != max_e[1][v][i])
				loopless = max(loopless, max_e[1][v][i]);
			u = anc[v][i];
		}
	}
	return max(loopmax, loopless);
}
void solve() {
	ll MST = kruskal();
	deep[1] = 1;
	DFS(1, 0);
	ll ans = ll_INF, t;
	for (int i = 0; i < m; i++) {
		if (!ori[i].use) {
			t = change(ori[i].u, ori[i].v, ori[i].val);
			if (t != -1)
				ans = _min(ans, MST - t + ori[i].val);
		}
	}
	printf("%lld", ans);
}
```

## 树

### 树链剖分

```c++
int siz[MX], fa[MX], son[MX], dep[MX], top[MX];
int dfn[MX], [MX], trcnt = 0;//以DFS序作为线段树节点,real[i]表示线段树节点i的实际编号
void DFS1(int u) {
    siz[u] = 1;
    son[u] = 0;
    for(int i = head[u]; i; i = e[i]._next) {
        int v = e[i].v;
        if(v != fa[u]) {
            fa[v] = u;
            dep[v] = dep[u] + 1;
            DFS1(v);
            siz[u] += siz[v];
            if(son[u] == 0 || siz[v] > siz[son[u]])
                son[u] = v;
        }
    }
}
void DFS2(int u, int tp) {
    dfn[u] = ++trcnt;
    real[trcnt] = u;
    top[u] = tp;
    if(son[u])
        DFS2(son[u], tp);
    for(int i = head[u]; i; i = e[i]._next) {
        int v = e[i].v;
        if(v != fa[u] && v != son[u])
            DFS2(v, v);
    }
}
```

### LCA

#### 倍增做法

```c++
int anc[MX][25] = {0}, dep[MX] = {0};
void DFS(int u, int fa) {
    dep[u] = dep[fa] + 1;
    anc[u][0] = fa;
    for(int i = 1; i <= 20; i++)
        anc[u][i] = anc[anc[u][i - 1]][i - 1];
    for(int i = head[u]; i; i = e[i]._next)
        if(e[i].v != fa)
            DFS(e[i].v, u);
}
int lca(int x, int y) {
    if(dep[x] < dep[y])
        swap(x, y);
    for(int i = 20; i >= 0; i--)
        if(dep[y] <= dep[anc[x][i]])
            x = anc[x][i];
    if(x == y)
        return x;
    for(int i = 20; i >= 0; i--) {
        if(anc[x][i] != anc[y][i]) {
            x = anc[x][i];
            y = anc[y][i];
        }
    }
    return anc[x][0];
}
```

#### 树链剖分做法

```c++
int lca(int x, int y) {
    while(top[x] != top[y]) {
        if(dep[top[x]] > dep[top[y]])
            swap(x, y);
        y = fa[top[y]];
    }
    return dep[x] > dep[y] ? y : x;
}
```

### 树上区间操作

基于线段树和树链剖分，线段树部分只需改动建树操作为 `tr[i].val=a[dfn[l]];  `

#### 修改子树内所有节点

```c++
void tree_add(int u,ll k){
	int begin=dfn[u];
	int end=begin+size[u]-1;
	update(begin,end,1,k);
}
```

#### 查询子树内的所有节点

```c++
ll tree_sum(int u) {
    int begin = dfn[u];
    int end = begin + siz[u] - 1;
    ll sum = query(1, begin, end) % MOD;
    return sum;
}
```

#### 修改链上的所有节点

```c++
void line_add(int u, int v, ll k) {
    while(top[u] ^ top[v]) {
        if(dep[top[u]] < dep[top[v]])
            swap(u, v);
        update(1, dfn[top[u]], dfn[u], k);
        u = fa[top[u]];
    }
    if(dep[u] > dep[v])
        swap(u, v);
    update(1, dfn[u], dfn[v], k);
}
```

#### 查询链上的所有节点

```c++
ll line_sum(int u, int v) {
    ll sum = 0;
    while(top[u] ^ top[v]) {
        if(dep[top[v]] > dep[top[u]])
            swap(u, v);
        sum += query(1, dfn[top[u]], dfn[u]);
        sum %= MOD;
        u = fa[top[u]];
    }
    if(dep[u] > dep[v])
        swap(u, v);
    sum += query(1, dfn[u], dfn[v]);
    sum %= MOD;
    return sum;
}
```

## 网络流

请注意初始条件下 **cnt=1**

### 最大流

建边时反向边的流量取0

#### Dinic

```c++
int dep[MX], nowe[MX], st, ed;
queue<int> q;
bool BFS() {
    for(int i = 0; i <= n; i++)
        dep[i] = 0;
    dep[st] = 1;
    q.push(st);
    while(!q.empty()) {
        int u = q.front();
        q.pop();
        for(int i = head[u]; i; i = e[i]._next) {
            int v = e[i].v;
            if(e[i].val > 0 && dep[v] == 0) {
                dep[v] = dep[u] + 1;
                q.push(v);
            }
        }
    }
    for(int i = 0; i <= n; i++)
        nowe[i] = head[i];
    return dep[ed];
}
ll DFS(int u, ll flow) {
    if(u == ed)
        return flow;
    ll last = flow;
    for(int i = nowe[u]; i; i = e[i]._next) {
        nowe[u] = i;
        int v = e[i].v;
        if(dep[v] == dep[u] + 1 && e[i].val) {
            ll nflow = DFS(v, _min(last, e[i].val));
            if(nflow > 0) {
                e[i].val -= nflow, e[i ^ 1].val += nflow;
                last -= nflow;
                if(last == 0)
                    return flow;
            }
        }
    }
    if(last == flow)
        dep[u] = 0;
    return flow - last;
}
ll Dinic() {
    ll nowflow = 0, maxflow = 0;
    while(BFS())
        while(nowflow = DFS(st, ll_INF))
            maxflow += nowflow;
    return maxflow;
}
```

#### LSAP

```c++
int dep[MX], gap[MX], nowe[MX], st, ed;
queue<int> q;
void BFS() {
    for(int i = 0; i <= n; i++)
        gap[i] = dep[i] = 0;
    dep[ed] = 1;
    gap[1]++;
    q.push(ed);
    while(!q.empty()) {
        int u = q.front();
        q.pop();
        for(int i = head[u]; i; i = e[i]._next) {
            int v = e[i].v;
            if(dep[v] == 0) {
                dep[v] = dep[u] + 1;
                gap[dep[v]]++;
                q.push(v);
            }
        }
    }
}
ll DFS(int u, ll flow) {
    if(u == ed)
        return flow;
    ll last = flow;
    for(int i = head[u]; i; i = e[i]._next) {
        nowe[u] = i;
        int v = e[i].v;
        if(dep[v] == dep[u] - 1 && e[i].val) {
            ll nflow = DFS(v, _min(last, e[i].val));
            if(nflow > 0) {
                e[i].val -= nflow, e[i ^ 1].val += nflow;
                last -= nflow;
                if(last == 0)
                    return flow;
            }
        }
    }
    gap[dep[u]]--;
    if(gap[dep[u]] == 0)
        dep[st] = n + 10;
    gap[++dep[u]]++;
    return flow - last;
}
ll lSAP() {
    ll maxflow = 0;
    BFS();
    while(dep[st] <= n) {
        for(int i = 0; i <= n; i++)
            nowe[i] = head[i];
        maxflow += DFS(st, ll_INF);
    }
    return maxflow;
}
```

### 最小费用最大流

建边时反向边的费用取相反数，流量取0

#### Dinic

```
int nowe[MX], vis[MX], st, ed;
ll dis[MX], mincost = 0;
queue<int> q;
bool SPFA() {
    for (int i = 0; i <= n; i++)
        vis[i] = 0, dis[i] = ll_INF;
    vis[st] = 1;
    dis[st] = 0;
    q.push(st);
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        vis[u] = 0;
        for (int i = head[u]; i; i = e[i]._next) {
            int v = e[i].v;
            if (e[i].val > 0 && dis[v] > dis[u] + e[i].cost) {
                dis[v] = dis[u] + e[i].cost;
                if (vis[v] == 0) {
                    vis[v] = 1;
                    q.push(v);
                }
            }
        }
    }
    for (int i = 0; i <= n; i++)
        nowe[i] = head[i];
    return dis[ed] != ll_INF;
}
ll DFS(int u, ll flow) {
    vis[u] = 1;
    if (u == ed)
        return flow;
    ll last = flow;
    for (int i = nowe[u]; i; i = e[i]._next) {
        nowe[u] = i;
        int v = e[i].v;
        if ((vis[v] == 0 || v == ed) && dis[v] == dis[u] + e[i].cost && e[i].val) {
            ll nflow = DFS(v, _min(last, e[i].val));
            if (nflow > 0) {
                mincost += e[i].cost * nflow;
                e[i].val -= nflow, e[i ^ 1].val += nflow;
                last -= nflow;
                if (last == 0)
                    return flow;
            }
        }
    }
    if (last == flow)
        dis[u] = ll_INF;
    return flow - last;
}
void Min_Cost_Dinic() {
    ll nowflow = 0, maxflow = 0;
    while (SPFA())
        while (nowflow = DFS(st, ll_INF))
            maxflow += nowflow;
    cout << maxflow << " " << mincost << endl;
}
```

## 二分图

### 最大匹配

匹配边数最多，每个点最多可匹配一次

#### 最大流做法

![image-20211116210953397](C:\Users\12038\AppData\Roaming\Typora\typora-user-images\image-20211116210953397.png)

建立超级源点和超级汇点，源点到x连流量为1，x到y每条正边流量为1，最后流入超级汇点的即为最大匹配数，枚举所有正边看剩余流量即可确定是否为选用的匹配边。

```c++
for(int i = 1; i <= n; i++) {
    addedge(0, i, 1);
    addedge(i, 0, 0);
}
for(int i = 1; i <= m; i++) {
    addedge(n + i, n + m + 1, 1);
    addedge(n + m + 1, n + i, 0);
}
for(int i = 0; i < f; i++) {
    cin >> u >> v;
    addedge(u, n + v, 1);
    addedge(n + v, u, 0);
}
```
