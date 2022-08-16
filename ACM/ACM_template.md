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
#define _chmax(a, b) (a = max(a, (b)))
#define _chmin(a, b) (a = min(a, (b)))
#define _abs(a) ((a) > 0 ? (a) : -(a))

inline int fsign(double x) {
    return ((fabs(x) < EPS) ? 0 : ((x < 0) ? -1 : 1));
}
inline ll _r() {
    ll x = 0, f = 1;
    char c = getchar();
    while (c > '9' || c < '0') { if (c == '-')f = -1; c = getchar(); }
    while (c >= '0' && c <= '9')x = (x << 3) + (x << 1) + (c ^ 48), c = getchar();
    return f == -1 ? -x : x;
}

const int MOD = 1e9+7;
const int MX = 1000010;
void Init() {

}

void solve() {

}
int main() {
    std::ios::sync_with_stdio(false);
#ifdef LOCAL
    freopen("./Input_data/data.in", "r", stdin);
    freopen("./Output_data/data.out", "w", stdout);
#endif
    int t = 1;
    //cin>> t;
    while (t--) {
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

## Cout格式化输出

### 全局方式

#### 设置有效数位

​	有效数字的函数不会保留没用的0有效位

```c++
cout.precision(2);
```

#### 设置有效数位显示后缀零

```
cout.setf(ios::showpoint);
cout.precision(4);
```

#### 设置小数位数

```c++
cout.flags(ios::fixed);
cout.precision(2);
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

  $ O(log\ n)$  二分第一个 $ \geq x $(lower) 或者   $ > x $ (upper)  的地址，在不存在对应值时都为右端点


如果数组是从小到大排序

​	那么`lower_bound(b,b+n,a[i])-b `: 输出b数组中第一个>=a[i]的值的下标

​	那么`upper_bound(b,b+n,a[i])-b `: 输出b数组中第一个>a[i]的值的下标

如果数组是从大到小排序

​	那么`lower_bound(b,b+n,a[i],greater<int>())-b `: 输出b数组中第一个<=a[i]的值的下标

​	那么`upper_bound(b,b+n,a[i],greater<int>())-b `: 输出b数组中第一个<a[i]的值的下标

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

## unordered_map

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
		swap(xx, yy);
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
class MonotoneQueue {
public:
    struct Data {
        int time, val;
    };
    int max_len;  // 单调队列最大长度
    deque<Data> q;
    void push(int val, int time = 0) {
        if(q.empty())
            q.push_back({time, val});
        else {
            // 保证单调性
            while(!q.empty() && q.back().val > val) {
                q.pop_back();
            }
            q.push_back({time, val});
            // 控制先前元素出栈
            if(max_len)
                while(!q.empty() && time - q.front().time >= max_len)
                    q.pop_front();
        }
    }
    int front() {
        return q.front().val;
    }
    void clear() {
        q.clear();
    }
} q;
```

## 单调栈

```c++
class MonotoneStack {
public:
    int sta[10000];
    int head;
    void push(int val) {
        if(head == 0)
            sta[head++] = val;
        else {
            // 保证单调性
            while(head >= 0 && sta[head] > val)
                head--;
            sta[head++] = val;
        }
    }
    int top() {
        return sta[head];
    }
    void clear() {
        head = 0;
    }
} sta;
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
		for (int j = 1; j <= n - (1 << i) + 1; j++) {
			ST[j][i] = _max(ST[j][i - 1], ST[j + (1 << (i - 1))][i -1]);
		}
	}
}
ll query_ST(int l, int r) {
	int k = Log2[r - l + 1];
	return _max(ST[l][k], ST[r - (1 << k) + 1][k]);
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

### 结构体版

```c++
struct Tree {
	int l, r;
	ll val;
	ll add;
}tr[MX << 2];
ll num[MX] = { 0 };
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
		tr[i].val = num[l];
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

### 数组简化版

只能对于长度为 $2^n$ 的数据，不足时需要补齐。单点修改RMQ有奇效

```

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
                if(num[i][j] != 0 || num[j][i] != 0)
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

### 分数

```c++
struct Frac {
    int up, down;
    Frac() {
        up = 0, down = 1;
    }
    Frac(int val) {
        up = val, down = 1;
    }
    Frac(int up, int down) {
        up = up, down = down;
        clear();
    }
    bool legal() {
        return down == 0;
    }
    void clear() {
        int _gcd = gcd(up, down);
        up /= _gcd, down /= _gcd;
    }
    double get_double() {
        return (double)up / down;
    }
    Frac operator+(const Frac x) const {
        Frac c;
        c.up = up * x.down + down * x.up;
        c.down = down * x.down;
        c.clear();
        return c;
    }
    Frac operator-(const Frac x) const {
        Frac c;
        c.up = up * x.down - down * x.up;
        c.down = down * x.down;
        c.clear();
        return c;
    }
    Frac operator*(const Frac x) const {
        Frac c;
        c.up = up * x.up;
        c.down = down * x.down;
        c.clear();
        return c;
    }
    ll operator%(const int x) const {  // x需为质数
        ll _gcd = gcd(down, x);
        if(_gcd != 1)  //不存在
            return -1;
        return (ll)up * _pow(down, x - 2) % x;
    }
    bool operator==(const Frac x) const {
        return up == x.up && down == x.down;
    }
    friend ostream& operator<<(ostream& out, const Frac& f) {
        if(f.up == 0)
            cout << 0;
        else if(f.up == 1)
            cout << f.up;
        else
            cout << f.up << '/' << f.down;
        return out;
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
			if (pri[j] * i >= range)break;
			not_pri[pri[j] * i] = 1;
			if (i % pri[j] == 0)break;
		}
	}
}
```

### 质因数分解

`p[i]`:质因数表 , `pcnt[i]`:对应质因数的次数

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
ll inv[MX] = {0};
void get_inv(int range) {
    inv[1] = 1;
    for(int i = 2; i < range; i++)
        inv[i] = ((MOD - MOD / i) * inv[MOD % i]) % MOD;
}
```

#### 任意 $n$ 个数的逆元

`a[i]`:原数组,下标从 1 开始,    `s[i]`:前缀积,    `s_inv[i]`:前缀积的逆元

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

也可以顺势计算普通阶乘 

```c++
ll inv[MX],fac_inv[MX];
void get_fac_inv(int range) {
	fac[0] = 1;
	for (int i = 1; i <= range; i++)
		fac[i] = (fac[i - 1] * i) % MOD;
	fac_inv[range] = _pow(fac[range], MOD - 2);
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

对较小质数$ p $ 取模，要求 $ p \leq 10^6 $
$$
{n \choose m}={n\mod p \choose m\mod p} \times {n/p \choose m/p}
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



## 常用不等式

### 均值不等式

$$
H_n \le G_n \le A_n \le Q_n \ ,\  \mbox{当且仅当各元素相等时取等}
$$

$$
\displaystyle
&H_n&=&\frac{n}{\sum_{i=1}^{n}\frac{1}{x_i}} &=&\frac{1}{\frac{1}{x_1}+\frac{1}{x_2}+\dots+\frac{1}{x_n}}\\
&G_n&=&\sqrt[n]{\prod_{i=1}^{n}x_i} &=&\sqrt[n]{x_1x_2\dots x_n} \\
&A_n&=&\frac{\sum_{i=1}^{n}x_i}{n} &=&\frac{x_1+x_2+\dots+x_n}{n}\\
&Q_n&=&\sqrt\frac{\sum_{i=1}^{n}x_i^2}{n} &=&\sqrt\frac{x_1^2+x_2^2+\dots+x_n^2}{n}\\
$$



#### 二元均值不等式

$$
\frac{2}{\frac1{a}+\frac1{b}}\le\sqrt{ab}\le\frac{a+b}{2}\le\sqrt{\frac{a^2+b^2}{2}}
$$

### 柯西不等式

$$

$$

## 应用专题

### 整数分拆问题

指将一个正整数表示为若干个正整数的和，其中主要讨论无序分拆的问题，即不考虑求和顺序（将 $3=1+2$ 与 $3=2+1$ 视为同种分拆方式）

#### 动态规划做法

`dp[i]` 表示在

# 计算几何

## 总模板

```c++
namespace Geometry {
    // 二维点类
    struct Point {
        double x, y;
        Point() {
            x = y = 0;
        }
        Point(double X, double Y) {
            x = X, y = Y;
        }
        double get_len() {
            return sqrt((double)x * x + y * y);
        }
        Point operator+(const Point& b) const {
            return Point(x + b.x, y + b.y);
        }
        Point operator-(const Point& b) const {
            return Point(x - b.x, y - b.y);
        }
        Point operator*(const double c) const {
            return Point(x * c, y * c);
        }
        Point operator/(const double c) const {
            return Point(x / c, y / c);
        }
        bool operator==(const Point& b) const {
            return fsign(x - b.x) == 0 && fsign(y - b.y) == 0;
        }
        bool operator!=(const Point& b) const {
            return fsign(x - b.x) != 0 || fsign(y - b.y) != 0;
        }
        bool operator<(const Point& b) const {
            if(fsign(x - b.x) == 0)
                return fsign(y - b.y) == -1;
            return fsign(x - b.x) == -1;
        }
        bool operator>(const Point& b) const {
            if(fsign(x - b.x) == 0)
                return fsign(y - b.y) == 1;
            return fsign(x - b.x) == 1;
        }
        //逆时针旋转theta角
        Point rotate(const double& theta) const {
            Point c;
            c.x = x * cos(theta) - y * sin(theta);
            c.y = y * cos(theta) + x * sin(theta);
            return c;
        }
        //逆时针90度
        Point rotate_inv90() const {
            return Point(-y, x);
        }
        //顺时针90度
        Point rotate_clk90() const {
            return Point(y, -x);
        }
        // 取模的平方
        double norm_square() const {
            return x * x + y * y;
        }
        // 取模
        double norm_sqrt() const {
            return sqrt(x * x + y * y);
        }
    };
    // 求象限
    int quad(Point p) {
        return ((p.y < 0) ? 1 : 3) + ((p.x < 0) ^ (p.y < 0));
    }
    // 点乘
    double dot(Point a, Point b) {
        return a.x * b.x + a.y * b.y;
    }
    // 叉乘
    double cross(Point a, Point b) {
        return a.x * b.y - a.y * b.x;
    }
    // 求向量夹角cos
    double get_angel_cos(Point a, Point b) {
        return dot(a, b) / a.norm_sqrt() / b.norm_sqrt();
    }
    // 欧几里得距离
    double e_dis(Point a, Point b) {
        return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
    }
    // 曼哈顿距离
    double m_dis(Point a, Point b) {
        return fabs(a.x - b.x) + fabs(a.y - b.y);
    }
    // 切比雪夫距离
    double c_dis(Point a, Point b) {
        return max(fabs(a.x - b.x), fabs(a.y - b.y));
    }
    // 二维向量类
    typedef Point Vector2D;
    // 以先x后y比较,小值在前
    bool compare_x(Point a, Point b) {
        return (fsign(a.x - b.x) == 0) ? (fsign(a.y - b.y) < 0) : (fsign(a.x - b.x) < 0);
    }
    // 以先y后x比较,小值在前
    bool compare_y(Point a, Point b) {
        return (fsign(a.y - b.y) == 0) ? (fsign(a.x - b.x) < 0) : (fsign(a.y - b.y) < 0);
    }
    // 极角排序叉积象限比较法,小值在前
    bool compare_angle(Point a, Point b) {
        if(quad(a) != quad(b))
            return quad(a) < quad(b);
        if(cross(a, b) != 0)
            return cross(a, b) > 0;
        return a.x < b.x;
    }
    // 判定两向量关系,<0逆反
    int position_vector_vector(Vector2D a, Vector2D b) {
        if(fsign(cross(a, b)) == -1)  // a在b的逆时针方向（左侧）
            return -2;
        if(fsign(cross(a, b)) == 1)  // a在b的顺时针方向（右侧）
            return 2;
        if(fsign(dot(a, b)) == -1)  // ab反向
            return -1;
        if(fsign(a.norm_square() - b.norm_square()) == -1)  // ab同向,且b长于a
            return 1;
        else  // ab同向,且b短于a,b在a上
            return 0;
    }
    // 判定三点关系(base->b,base->c)
    int position_point_point(Point base, Point b, Point c) {
        return position_vector_vector(b - base, c - base);
    }
    // 求两个向量围起来的三角形面积
    double get_S(Vector2D a, Vector2D b) {
        return fabs(cross(a, b)) / 2;
    }
    // 求给出三边长的三角形面积
    double get_S(double a, double b, double c) {
        double s = (a + b + c) / 2;
        return sqrt(s * (s - a) * (s - b) * (s - c));
    }
    // 直线类
    struct Line {
        Point s, t;
        Vector2D normal;  //方向向量
        Line() {
        }
        Line(const Point st, const Point ed) {
            set(st, ed);
        }
        void set(const Point a, const Point b) {
            s = a, t = b;
            update_normal();
        }
        void update_normal() {
            normal = t - s;
        }
        bool operator<(Line a) const {
            if(normal != a.normal)
                return normal < a.normal;
            else if(s != a.s)
                return s < a.s;
            return t < a.t;
        }
        //反向
        void inv() {
            swap(s, t);
            normal.x = -normal.x, normal.y = -normal.y;
        }
    };
    // 线段
    typedef Line Seg;
    // 判定点与直线关系 2:左,1:在直线上,0:右
    int position_point_line(Point p, Line l) {
        int res = fsign(cross(p - l.s, l.normal));
        if(res == 1)
            return 0;
        if(res == 0)
            return 1;
        return 2;
    }
    // 判定点与线段关系 3:左,2:共线但不在线段上,1:在线段上,0:右
    int position_point_seg(Point p, Seg l) {
        int res = fsign(cross(p - l.s, l.normal));
        if(res == 1)
            return 0;
        if(res == 0) {
            if(p.x >= min(l.s.x, l.t.x) && p.x <= max(l.s.x, l.t.x) && p.y >= min(l.s.y, l.t.y) && p.y <= max(l.s.y, l.t.y))
                return 1;
            else
                return 2;
        }
        return 3;
    }
    // 判定直线(线段)是否垂直
    bool is_vertical_line(const Line a, const Line b) {
        return fsign(dot(a.normal, b.normal)) == 0;
    }
    // 判定直线(线段)是否平行
    bool is_parallel_line(const Line a, const Line b) {
        return fsign(cross(a.normal, b.normal)) == 0;
    }
    // 判定直线(线段)是否共线
    bool is_same_line(const Line a, const Line b) {
        return is_parallel_line(a, b) && is_parallel_line(Line(a.s, b.t), Line(b.s, a.t));
    }
    // 判定直线是否相交
    bool is_corss_line(Line a, Line b) {
        return !is_parallel_line(a, b);
    }
    // 判定线段是否相交
    bool is_corss_seg(Seg a, Seg b) {
        if(position_point_point(a.s, a.t, b.s) * position_point_point(a.s, a.t, b.t) > 0)
            return 0;
        if(position_point_point(b.s, b.t, a.s) * position_point_point(b.s, b.t, a.t) > 0)
            return 0;
        return 1;
    }
    // 求p在直线上的投影(垂点)
    Point get_point_projection(Point p, Line line) {
        double num_t = dot(p - line.s, line.normal) / line.normal.norm_square();
        return line.s + line.normal * num_t;
    }
    // 求线段交点,不包含无交点情况
    Point get_point_corss_seg_seg(Seg a, Seg b) {
        double d1 = fabs(cross(b.normal, a.s - b.s));
        double d2 = fabs(cross(b.normal, a.t - b.s));
        if(fsign(d1 + d2) == 0)
            return b.s;
        double num_t = d1 / (d1 + d2);
        return a.s + a.normal * num_t;
    }
    // 求直线交点,不包含无交点情况
    Point get_point_corss_line_line(Line a, Line b) {
        double d1 = cross(a.normal, b.normal);
        double d2 = cross(a.normal, a.t - b.s);
        if(fsign(d1 + d2) == 0)
            return b.s;
        double num_t = d2 / d1;
        return b.s + b.normal * num_t;
    }
    // 求点到直线距离
    double get_dis_point_line(Point p, Line l) {
        return fabs(cross(l.normal, p - l.s) / l.normal.norm_sqrt());
    }
    // 求点到线段距离
    double get_dis_point_seg(Point p, Seg l) {
        if(fsign(dot(l.normal, p - l.s)) < 0)
            return (p - l.s).norm_sqrt();
        if(fsign(dot(l.normal, p - l.t)) > 0)
            return (p - l.t).norm_sqrt();
        return get_dis_point_line(p, l);
    }
    // 求直线之间的距离
    double get_dis_line_line(Line a, Line b) {
        if(is_corss_line(a, b))
            return 0;
        return get_dis_point_line(b.t, a);
    }
    // 求线段之间的距离
    double get_dis_seg_seg(Seg a, Seg b) {
        if(is_corss_seg(a, b))
            return 0;
        double dis1 = min(get_dis_point_seg(b.s, a), get_dis_point_seg(b.t, a));
        double dis2 = min(get_dis_point_seg(a.s, b), get_dis_point_seg(a.t, b));
        return min(dis1, dis2);
    }
    // 求线段到直线距离
    double get_dis_seg_line(Seg seg, Line line) {
        if(is_parallel_line(seg, line))
            return get_dis_point_line(seg.s, line);
        if(position_point_line(seg.s, line) * position_point_line(seg.t, line) <= 0)
            return 0;
        return min(get_dis_point_line(seg.s, line), get_dis_point_line(seg.t, line));
    }
    // 多边形类
    struct Polygon {
        vector<Point> poi;
        int n;
        Polygon() {
            clear();
        }
        Polygon(vector<Point>& points) {
            set(points);
        }
        void set(vector<Point>& points) {
            poi = points;
            n = poi.size();
        }
        void clear() {
            poi.clear();
            n = 0;
        }
        void add_point(Point p) {
            poi.push_back(p);
            n++;
        }
        void add_point(double x, double y) {
            poi.push_back(Point(x, y));
            n++;
        }
        //求多边形面积
        double get_S() {
            if(n == 0)
                return 0;
            double res = cross(poi[0], poi[n - 1]);
            for(int i = 1; i < n; i++) {
                res += cross(poi[i], poi[i - 1]);
            }
            return fabs(res) / 2;
        }
    };
    // 判定点与多边形关系 2:点在多边形里,1:点在多边形边上,0:点在多边形外
    int position_polygon_point(Polygon& poly, Point p) {
        bool x = false;
        poly.poi.push_back(poly.poi[0]);
        for(int i = 0; i < poly.n; i++) {
            Point a = poly.poi[i] - p, b = poly.poi[i + 1] - p;
            if(fsign(abs(cross(a, b))) == 0 && fsign(dot(a, b)) < 1)
                return 1;
            if(fsign(a.y - b.y) > 0)
                swap(a, b);
            if(fsign(a.y) < 1 && fsign(b.y) > 0 && fsign(cross(a, b)) > 0)
                x = !x;
        }
        poly.poi.pop_back();
        return (x ? 2 : 0);
    }
    // 判定是否为凸包
    bool is_convex(Polygon& poly) {
        for(int i = 0; i < poly.n; i++) {
            if(position_point_point(poly.poi[i], poly.poi[(i + 1 >= poly.n) ? (i + 1 - poly.n) : (i + 1)], poly.poi[(i + 2 >= poly.n) ? (i + 2 - poly.n) : (i + 2)]) == -2)
                return 0;
        }
        return 1;
    }
    // 从点集构造凸包,若不想要在凸包的边上有点改为<=0
    Polygon build_convex(vector<Point> pointSet) {
        int now = 0, n = pointSet.size();
        sort(pointSet.begin(), pointSet.end());
        vector<Point> res(n << 1);
        for(int i = 0; i < n; res[now++] = pointSet[i++]) {
            while(now > 1 && fsign(cross(res[now - 1] - res[now - 2], pointSet[i] - res[now - 2])) < 0)
                now--;
        }
        int t = now;
        for(int i = n - 2; i >= 0; res[now++] = pointSet[i--]) {
            while(now > t && fsign(cross(res[now - 1] - res[now - 2], pointSet[i] - res[now - 2])) < 0)
                now--;
        }
        res.resize(now - 1);
        return Polygon(res);
    }
    // 求凸包直径,旋转卡壳
    double get_diameter_convex(Polygon& poly) {
        if(poly.n == 2)
            return e_dis(poly.poi[0], poly.poi[1]);
        int a = 0, b = 0;
        for(int i = 1; i < poly.n; i++) {
            if(poly.poi[i] < poly.poi[a])
                a = i;
            if(poly.poi[i] > poly.poi[b])
                b = i;
        }
        double max_dis = e_dis(poly.poi[a], poly.poi[b]);
        int now_a = a, now_b = b;
        while(now_a != b || now_b != a) {
            double now_dis = e_dis(poly.poi[now_a], poly.poi[now_b]);
            max_dis = max(max_dis, now_dis);
            int next_a = (now_a + 1 == poly.n) ? 0 : now_a + 1;
            int next_b = (now_b + 1 == poly.n) ? 0 : now_b + 1;
            if(fsign(cross(poly.poi[next_a] - poly.poi[now_a], poly.poi[next_b] - poly.poi[now_b])) < 0)
                now_a = next_a;
            else
                now_b = next_b;
        }
        return max_dis;
    }
    // 求凸包沿直线方向向量切割后左侧的凸包
    Polygon get_convex_cut(Polygon& poly, Line& l) {
        vector<Point> res;
        for(int i = 0; i < poly.n; i++) {
            Point A = poly.poi[i];
            Point B = poly.poi[(i + 1) == poly.n ? 0 : (i + 1)];
            if(position_point_point(l.s, l.t, A) != -2)
                res.push_back(A);
            if(position_point_point(l.s, l.t, A) * position_point_point(l.s, l.t, B) < 0)
                res.push_back(get_point_corss_seg_seg(Line(A, B), l));
        }
        return Polygon(res);
    }
    // 圆类
    struct Circle {
        Point o;
        double r;
        Circle() {
            clear();
        }
        Circle(Point center, double radix) {
            set(center, radix);
        }
        void clear() {
            o.x = o.y = 0, r = 0;
        }
        void set(Point center, double radix) {
            o = center, r = radix;
        }
        // 圆面积
        double get_S() {
            return PI * r * r;
        }
        // 根据极角获得对应点
        Point get_point(double angle) {
            return Point(o.x + r * cos(angle), o.y + r * sin(angle));
        }
        // 根据向量方向获得对应点
        Point get_point(Vector2D dir) {
            return o + dir / dir.norm_sqrt() * r;
        }
    };
    // 判定点与圆的关系 2:在圆内,1:在圆的边上,0:在圆外
    int position_circle_point(Circle& c, Point p) {
        double dis = e_dis(c.o, p) - c.r;
        if(fsign(dis) == 0)
            return 1;
        else if(fsign(dis) == 1)
            return 0;
        else
            return 2;
    }
    // 判定直线与圆的关系(交点个数) 2:相交,1:相切,0:相离
    int position_circle_line(Circle& c, Line& l) {
        double dis = get_dis_point_line(c.o, l) - c.r;
        if(fsign(dis) == 0)
            return 1;
        else if(fsign(dis) == 1)
            return 0;
        else
            return 2;
    }
    // 判定线段与圆的关系(交点个数) 2:相交,1:相切,0:相离
    int position_circle_seg(Circle& c, Seg& l) {
        double dis = get_dis_point_seg(c.o, l) - c.r;
        if(fsign(dis) == 0)
            return 1;
        else if(fsign(dis) == 1)
            return 0;
        else
            return 2;
    }
    // 判定圆与圆的关系(切线数量) 4:相离,3:外接,2:相交,1:内切,0:包含
    int position_circle_circle(Circle& c1, Circle& c2) {
        double dis = e_dis(c1.o, c2.o);
        if(fsign(dis - (c1.r + c2.r)) > 0)
            return 4;
        else if(fsign(dis - (c1.r + c2.r)) == 0)
            return 3;
        else if(fsign(dis - fabs(c1.r - c2.r)) == 0)
            return 1;
        else if(fsign(dis - fabs(c1.r - c2.r)) < 0)
            return 0;
        else
            return 2;
    }
    // 判定多边形与圆的关系 ,2:其他,1:内切,0:包含
    int position_polygon_circle(Polygon& poly, Circle& c) {
        if(position_polygon_point(poly, c.o) != 2)
            return 2;
        Line l;
        int pos, res = 0;
        for(int i = 0; i < poly.n; i++) {
            l.set(poly.poi[i], poly.poi[(i + 1 == poly.n) ? 0 : (i + 1)]);
            pos = fsign(c.r - get_dis_point_seg(c.o, Line(l)));
            if(pos == 1)
                return 2;
            else if(pos == 0)
                res = 1;
        }
        return res;
    }
    // 构建三角形内切圆,可以处理三点共线的情况
    Circle build_circle_triangle_incision(Point p1, Point p2, Point p3) {
        Circle res;
        double a = (p2 - p3).norm_sqrt(), b = (p3 - p1).norm_sqrt(), c = (p1 - p2).norm_sqrt();
        double S = fabs(cross(p1 - p2, p2 - p3));
        double C = a + b + c;
        res.r = S / C;
        res.o.x = (a * p1.x + b * p2.x + c * p3.x) / C;
        res.o.y = (a * p1.y + b * p2.y + c * p3.y) / C;
        return res;
    }
    // 构建三角形外接圆,不可以处理三点共线的情况
    Circle build_circle_triangle_external(Point p1, Point p2, Point p3) {
        Point mid1 = (p2 + p1) / 2;
        Point mid2 = (p2 + p3) / 2;
        Line v1(mid1, mid1 + (p1 - p2).rotate_inv90());
        Line v2(mid2, mid2 + (p3 - p2).rotate_inv90());
        Point c = get_point_corss_line_line(v1, v2);
        return Circle(c, e_dis(c, p1));
    }
    // 求圆与直线交点
    vector<Point> get_point_corss_circle_line(Circle& c, Line l) {
        vector<Point> res;
        Point p = get_point_projection(c.o, l);
        int pos = fsign(e_dis(p, c.o) - c.r);
        if(pos < 0) {
            double num_t = sqrt(c.r * c.r - (p - c.o).norm_square()) / l.normal.norm_sqrt();
            res.push_back(p - l.normal * num_t);
            res.push_back(p + l.normal * num_t);
        }
        else if(pos == 0)
            res.push_back(p);
        return res;
    }
    // 求圆与圆交点
    vector<Point> get_point_corss_circle_circle(Circle& c1, Circle& c2) {
        vector<Point> res;
        int pos = position_circle_circle(c1, c2);
        if(pos == 0 || pos == 4)
            return res;
        double dis = e_dis(c2.o, c1.o);
        double angle_ori = atan2((c2.o - c1.o).y, (c2.o - c1.o).x);
        if(pos == 2) {
            double angle_tar = acos((c1.r * c1.r - c2.r * c2.r + dis * dis) / (2 * c1.r * dis));
            res.push_back(c1.get_point(angle_ori + angle_tar));
            res.push_back(c1.get_point(angle_ori - angle_tar));
        }
        else
            res.push_back(c1.get_point(angle_ori));
        return res;
    }
    // 求圆过某点的切线与圆的切点
    vector<Point> get_point_tangent_circle_point(Circle& c, Point p) {
        vector<Point> res;
        int pos = position_circle_point(c, p);
        if(pos == 0) {
            double dis = e_dis(c.o, p);
            double angle_tar = asin(c.r / dis);
            double angle_ori = atan2((c.o - p).y, (c.o - p).x);
            double len = sqrt(dis * dis - c.r * c.r);
            res.push_back(p + Vector2D(len * cos(angle_ori + angle_tar), len * sin(angle_ori + angle_tar)));
            res.push_back(p + Vector2D(len * cos(angle_ori - angle_tar), len * sin(angle_ori - angle_tar)));
        }
        else if(pos == 1)
            res.push_back(p);
        return res;
    }
    // 求两个圆的公切线
    vector<Line> get_line_tangent_circle_circle(Circle c1, Circle c2) {
        vector<Line> res;
        int exchange = 0;
        if(c1.r < c2.r) {
            swap(c1, c2);
            exchange = 1;
        }
        int pos = position_circle_circle(c1, c2);
        if(pos == 0)
            return res;
        double dis = e_dis(c1.o, c2.o);
        double angle_ori = atan2((c2.o - c1.o).y, (c2.o - c1.o).x);
        if(pos == 1) {
            res.push_back(Line(c1.get_point(angle_ori), c2.get_point(angle_ori)));
        }
        else {
            double angle_tar = acos((c1.r - c2.r) / dis);
            res.push_back(Line(c1.get_point(angle_ori + angle_tar), c2.get_point(angle_ori + angle_tar)));
            res.push_back(Line(c1.get_point(angle_ori - angle_tar), c2.get_point(angle_ori - angle_tar)));
            if(pos == 4) {
                angle_tar = acos((c1.r + c2.r) / dis);
                res.push_back(Line(c1.get_point(angle_ori + angle_tar), c2.get_point(angle_ori + angle_tar + PI)));
                res.push_back(Line(c1.get_point(angle_ori - angle_tar), c2.get_point(angle_ori - angle_tar + PI)));
            }
            else if(pos == 3) {
                res.push_back(Line(c1.get_point(angle_ori), c2.get_point(angle_ori + PI)));
            }
        }
        if(exchange)
            for(int i = 0; i < res.size(); i++)
                res[i].inv();
        return res;
    }
    // 构建过已知两点和半径的圆
    vector<Circle> build_circle_point_point_radix(Point p1, Point p2, double radix) {
        Circle c1(p1, radix), c2(p2, radix);
        vector<Point> newo = get_point_corss_circle_circle(c1, c2);
        vector<Circle> res;
        for(int i = 0; i < newo.size(); i++) {
            res.push_back(Circle(newo[i], radix));
        }
        return res;
    }
    // 求向量夹角AOB方向上OA到OB的有向扇形面积
    double get_s_sector_vectors(Vector2D OA, Vector2D OB, double r) {
        if(OA.norm_square() == 0 || OB.norm_square() == 0)
            return 0;
        double angle = acos(get_angel_cos(OA, OB));
        if(fsign(cross(OA, OB)) >= 0)
            return r * r * angle / 2;
        else
            return -r * r * angle / 2;
    }
    // 求角AOB方向上OA到OB的有向扇形面积
    double get_s_sector_points(Point O, Point A, Point B, double r) {
        return get_s_sector_vectors(A - O, B - O, r);
    }
    // 求两个圆的相交面积
    double get_s_circle_circle(Circle& c1, Circle& c2) {
        int pos = position_circle_circle(c1, c2);
        if(pos >= 3)
            return 0;
        if(pos <= 1)
            return (c1.r < c2.r) ? c1.get_S() : c2.get_S();
        Vector2D d = c2.o - c1.o;
        double dis = d.norm_sqrt(), dis2 = d.norm_square();
        double angle_1 = acos((dis2 + c1.r * c1.r - c2.r * c2.r) / (2 * dis * c1.r)) * 2;
        double angle_2 = acos((dis2 + c2.r * c2.r - c1.r * c1.r) / (2 * dis * c2.r)) * 2;
        return (angle_1 - sin(angle_1)) * c1.r * c1.r / 2 + (angle_2 - sin(angle_2)) * c2.r * c2.r / 2;
    }
    // 求三角形aob与圆的有向相交面积,o为圆心
    double get_s_triangle_circle(Point A, Point B, Circle& c) {
        Vector2D OA = A - c.o, OB = B - c.o;
        double res = cross(OA, OB), lena = OA.norm_square(), lenb = OB.norm_square(), r = c.r * c.r;
        if(fsign(res) == 0)
            return 0;
        if(fsign(max(lena, lenb) - r) <= 0)
            return res / 2;
        if(fsign(get_dis_point_seg(c.o, Line(A, B)) - c.r) >= 0)
            return get_s_sector_vectors(OA, OB, c.r);
        vector<Point> p = get_point_corss_circle_line(c, Line(A, B));
        Vector2D OE = p[0] - c.o, OF = p[1] - c.o;
        if(fsign(r - lena) > 0)
            return cross(OA, OF) / 2 + get_s_sector_vectors(OF, OB, c.r);
        if(fsign(r - lenb) > 0)
            return cross(OE, OB) / 2 + get_s_sector_vectors(OA, OE, c.r);
        return get_s_sector_vectors(OA, OE, c.r) + cross(OE, OF) / 2 + get_s_sector_vectors(OF, OB, c.r);
    }
    // 求多边形与圆的相交面积
    double get_s_polygon_circle(Polygon& poly, Circle& c) {
        if(poly.n < 3)
            return 0;
        if(position_polygon_circle(poly, c) != 2)
            return c.get_S();
        double res = 0;
        for(int i = 0; i < poly.n; i++) {
            int j = ((i + 1 == poly.n) ? 0 : i + 1);
            res += get_s_triangle_circle(poly.poi[i], poly.poi[j], c);
        }
        return res;
    }
};
using namespace Geometry;
```



## 二维点与向量

### 基本类

```c++
#define Vector2D Point
struct Point {
    double x, y;
    Point() {
        x = y = 0;
    }
    Point(double X, double Y) {
        x = X, y = Y;
    }
    double get_len() {
        return sqrt((double)x * x + y * y);
    }
    Point operator+(const Point& b) const {
        return Point(x + b.x, y + b.y);
    }
    Point operator-(const Point& b) const {
        return Point(x - b.x, y - b.y);
    }
    Point operator*(const double c) const {
        return Point(x * c, y * c);
    }
    Point operator/(const double c) const {
        return Point(x / c, y / c);
    }
    bool operator==(const Point& b) const {
        return fsign(x - b.x) == 0 && fsign(y - b.y) == 0;
    }
    bool operator!=(const Point& b) const {
        return fsign(x - b.x) != 0 || fsign(y - b.y) != 0;
    }
    bool operator<(const Point& b) const {
        if(fsign(x - b.x) == 0)
            return fsign(y - b.y) == -1;
        return fsign(x - b.x) == -1;
    }
    bool operator>(const Point& b) const {
        if(fsign(x - b.x) == 0)
            return fsign(y - b.y) == 1;
        return fsign(x - b.x) == 1;
    }
    //逆时针旋转theta角
    Vector2D rotate(const double& theta) const {
        Point c;
        c.x = x * cos(theta) - y * sin(theta);
        c.y = y * cos(theta) + x * sin(theta);
        return c;
    }
    //逆时针90度
    Point rotate_inv90() const {
        return Point(-y, x);
    }
    //顺时针90度
    Point rotate_clk90() const {
        return Point(y, -x);
    }
    // 取模的平方
    double norm_square() const {
        return x * x + y * y;
    }
    // 取模
    double norm_sqrt() const {
        return sqrt(x * x + y * y);
    }
    //求象限
    friend int quad(Point p) {
        return ((p.y < 0) ? 1 : 3) + ((p.x < 0) ^ (p.y < 0));
    }
    //点乘
    friend double dot(Point a, Point b) {
        return a.x * b.x + a.y * b.y;
    }
    //叉乘
    friend double cross(Point a, Point b) {
        return a.x * b.y - a.y * b.x;
    }
    //求向量夹角cos
    friend double get_angel_cos(Point a, Point b) {
        return (dot(a, b)) / (a.norm_sqrt() * b.norm_sqrt());
    }
    //判定两向量关系,<0逆反
    friend int position_vectors(Point a, Point b) {
        if(fsign(cross(a, b)) == -1)  // a在b的逆时针方向（左侧）
            return -2;
        if(fsign(cross(a, b)) == 1)  // a在b的顺时针方向（右侧）
            return 2;
        if(fsign(dot(a, b)) == -1)  // ab反向
            return -1;
        if(fsign(a.norm_square() - b.norm_square()) == -1)  // ab同向,且b长于a
            return 1;
        else  // ab同向,且b短于a,b在a上
            return 0;
    }
    //判定三点关系(base->b,base->c)
    friend int position_points(Point base, Point b, Point c) {
        return position_vectors(b - base, c - base);
    }
    //欧几里得距离
    friend double e_dis(Point a, Point b) {
        return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
    }
    //曼哈顿距离
    friend double m_dis(Point a, Point b) {
        return fabs(a.x - b.x) + fabs(a.y - b.y);
    }
    //切比雪夫距离
    friend double c_dis(Point a, Point b) {
        return max(fabs(a.x - b.x), fabs(a.y - b.y));
    }
};

```

### 面积

#### 三角形面积

```c++
double get_S(Point a, Point b) {
	return _abs(cross(a, b)) / 2;
}
```

#### 海伦公式

```c++
double get_S(double a, double b, double c) {
    double s = (a + b + c) / 2;
    return sqrt(s * (s - a) * (s - b) * (s * c));
}
```



### 角度

#### 旋转

计算 $\vec{v}=(x,y)$ 绕原点（起点）逆时针旋转 $\theta$ 后的向量

```c++
Vector2 rotate(Vector2& a, const double& theta) {
    Vector2 c;
    c.x = a.x * cos(theta) - a.y * sin(theta);
    c.y = a.y * cos(theta) + a.x * sin(theta);
    return c;
}
```

#### 极角排序

逆时针排序向量或点，其中弧度比较常数较小，叉积比较精度较高

##### 极角

计算 $\vec{v}=(x,y)$ 顺时针到 $x$ 轴正半轴方向转过的角度 $\theta -\pi$，值域为 $(-\pi,\pi\ ]$ 

```c++
double theta = atan2(x, y);
```

##### 弧度比较法

```c++
bool cmp(const Point& p1, const Point& p2) {
    return atan2(p1.y, p1.x) < atan2(p2.y, p2.x);
}
```

##### 叉积比较法

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

### 距离

#### 欧几里得距离 

$\sqrt{(x_1-x_2)^2+(y_1-y_2)}$

```c++
double e_dis(Point& a, Point& b) {
    return sqrt((ll)(a.x - b.x) * (a.x - b.x) + (ll)(a.y - b.y) * (a.y - b.y));
}
```

#### 曼哈顿距离

$|x_1-x_2|+|y_1-y_2|$

```c++
int m_dis(Point& a, Point& b) {
    return _abs(a.x - b.x) + _abs(a.y - b.y);
}
```

#### 切比雪夫距离

$max(|x_1-x_2|,|y_1-y_2|)$

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

### 平面最近点对

```
bool compare_y(Point a, Point b) {
    return (fsign(a.y - b.y) == 0) ? (fsign(a.x - b.x) < 0) : (fsign(a.y - b.y) < 0);
}
bool compare_x(Point a, Point b) {
    return (fsign(a.x - b.x) == 0) ? (fsign(a.y - b.y) < 0) : (fsign(a.x - b.x) < 0);
}
double closest_pair(vector<Point>::iterator now, int temp_n) {
    if(temp_n <= 1)
        return 1e50;
    int mid = temp_n >> 1;
    double x = now[mid].x;
    double dis = min(closest_pair(now, mid), closest_pair(now + mid, temp_n - mid));
    inplace_merge(now, now + mid, now + temp_n, compare_y);
    vector<Point> temp;
    for(int i = 0; i < temp_n; i++) {
        double nowx = now[i].x;
        double nowy = now[i].y;
        if(fsign(abs(nowx - x) - dis) >= 0)
            continue;
        for(int j = 0; j < temp.size(); j++) {
            double dx = nowx - temp[temp.size() - 1 - j].x;
            double dy = nowy - temp[temp.size() - 1 - j].y;
            if(fsign(dy - dis) >= 0)
                break;
            dis = min(dis, sqrt(dx * dx + dy * dy));
        }
        temp.push_back(now[i]);
    }
    return dis;
}
double closest_pair(vector<Point> points) {
    sort(points.begin(), points.end(), compare_x);
    return closest_pair(points.begin(), points.size());
}
```

## 二维直线

### 基本类

#### 点点式

```c++
struct Line {
    Point s, t;
    Vector2D normal;  //方向向量
    Line() {
    }
    Line(const Point st, const Point ed) {
        set(st, ed);
    }
    void set(const Point a, const Point b) {
        s = a, t = b;
        update_normal();
    }
    void update_normal() {
        normal = t - s;
    }
    bool operator==(Line a) const {
        return is_same_line((*this), a);
    }
    bool operator<(Line a) const {
        if(normal != a.normal)
            return normal < a.normal;
        else if(s != a.s)
            return s < a.s;
        return t < a.t;
    }
    friend bool is_vertical_line(const Line a, const Line b) {
        return fsign(dot(a.normal, b.normal)) == 0;
    }
    friend bool is_parallel_line(const Line a, const Line b) {
        return fsign(cross(a.normal, b.normal)) == 0;
    }
    friend bool is_same_line(const Line a, const Line b) {
        return is_parallel_line(a, b) && is_parallel_line(Line(a.s, b.t), Line(b.s, a.t));
    }
    // 求p在直线line上的投影(垂点)
    friend Point get_point_projection(Line line, Point p) {
        double num_t = dot(p - line.s, line.normal) / line.normal.norm_square();
        return line.s + line.normal * num_t;
    }
    // 判定线段是否相交
    friend bool is_corss_seg(Line a, Line b) {
        if(position_points(a.s, a.t, b.s) * position_points(a.s, a.t, b.t) > 0)
            return 0;
        if(position_points(b.s, b.t, a.s) * position_points(b.s, b.t, a.t) > 0)
            return 0;
        return 1;
    }
    // 求线段相交点
    friend Point get_point_corss(Line a, Line b) {
        double d1 = fabs(cross(b.normal, a.s - b.s));
        double d2 = fabs(cross(b.normal, a.t - b.s));
        if(fsign(d1 + d2) == 0)
            return b.s;
        double num_t = d1 / (d1 + d2);
        return a.s + a.normal * num_t;
    }
    // 求点到直线距离
    friend double get_dis_point_line(Line l, Point p) {
        return fabs(cross(l.normal, p - l.s) / l.normal.norm_sqrt());
    }
    // 求点到线段距离
    friend double get_dis_point_seg(Line l, Point p) {
        if(fsign(dot(l.normal, p - l.s)) < 0)
            return (p - l.s).norm_sqrt();
        if(fsign(dot(l.normal, p - l.t)) > 0)
            return (p - l.t).norm_sqrt();
        return get_dis_point_line(l, p);
    }
    // 求直线之间的距离
    friend double get_dis_lines(Line a, Line b) {
        if(is_corss_seg(a, b))
            return 0;
        return fabs(get_dis_point_line(a, b.t));
    }
    // 求线段之间的距离
    friend double get_dis_segs(Line a, Line b) {
        if(is_corss_seg(a, b))
            return 0;
        double dis1 = min(get_dis_point_seg(a, b.s), get_dis_point_seg(a, b.t));
        double dis2 = min(get_dis_point_seg(b, a.s), get_dis_point_seg(b, a.t));
        return min(dis1, dis2);
    }
};
```

#### 一般式

```c++
struct Line {
    // Ax+By+C=0
    ll A, B, C;
    void format() {
        if(A < 0 || (A == 0 && B < 0))
            A = -A, B = -B, C = -C;
        ll _gcd = gcd(gcd(abs(A), abs(B)), abs(C));
        A /= _gcd, B /= _gcd, C /= _gcd;
    }
    void set(Point a, Point b) {
        A = b.y - a.y;
        B = a.x - b.x;
        C = (ll)b.x * a.y - a.x * b.y;
        format();
    }
    bool operator==(const Line& a) const {
        return A == a.A && B == a.B && C == a.C;
    }
    bool operator<(const Line& a) const {
        if(A != a.A)
            return A < a.A;
        else if(B != a.B)
            return B < a.B;
        return C < a.C;
    }
    friend ostream& operator<<(ostream& out, const Line& x) {
        out << "L:" << x.A << "x";
        if(x.B >= 0)
            out << "+";
        out << x.B << "y";
        if(x.C >= 0)
            out << "+";
        out << x.C << "=0" << endl;
        return out;
    }
} line[MX];
```

### 平行于坐标轴的线段交点个数

```c++
struct EndPoint {
    Point p;
    int seg, kind;  //点类型
    EndPoint() {
    }
    EndPoint(Point p, int s, int kind): p(p), seg(s), kind(kind) {
    }
    bool operator<(EndPoint x) const {
        if(p.y == x.p.y)
            return kind < x.kind;
        else
            return p.y < x.p.y;
    }
};
int axis_segments_intersections(vector<Line> s) {
    int n = s.size();
    for(int i = 0; i < n; ++i)
        if((s[i].s.y > s[i].t.y) || (s[i].s.y == s[i].t.y && s[i].s.x > s[i].t.x))
            swap(s[i].s, s[i].t);
    EndPoint temp[n << 1];
    for(int i = 0; i < n; ++i) {
        if(s[i].s.x == s[i].t.x) {
            temp[i << 1] = EndPoint(s[i].s, i, 1);  //下
            temp[i << 1 | 1] = EndPoint(s[i].t, i, 4);  //上
        }
        else if(s[i].s.y == s[i].t.y) {
            temp[i << 1] = EndPoint(s[i].s, i, 2);  //左
            temp[i << 1 | 1] = EndPoint(s[i].t, i, 3);  //右
        }
    }
    sort(temp, temp + (n << 1));
    set<int> bst;
    int count = 0;
    for(int i = 0; i < (n << 1); ++i) {
        if(temp[i].kind == 1)
            bst.insert(temp[i].p.x);
        else if(temp[i].kind == 4)
            bst.erase(temp[i].p.x);
        else if(temp[i].kind == 2)
            count += distance(bst.lower_bound(s[temp[i].seg].s.x), bst.upper_bound(s[temp[i].seg].t.x));
    }
    return count;
}
```



## 多边形

### 基本类

```c++
struct Polygon {
    vector<Point> poi;
    int n;
    Polygon() {
        clear();
    }
    Polygon(vector<Point>& points) {
        set(points);
    }
    void set(vector<Point>& points) {
        poi = points;
        n = poi.size();
    }
    void clear() {
        poi.clear();
        n = 0;
    }
    void add_point(Point p) {
        poi.push_back(p);
        n++;
    }
    void add_point(double x, double y) {
        poi.push_back(Point(x, y));
        n++;
    }
    //求多边形面积
    double get_S() {
        if(n == 0)
            return 0;
        double res = cross(poi[0], poi[n - 1]);
        for(int i = 1; i < n; i++) {
            res += cross(poi[i], poi[i - 1]);
        }
        return fabs(res) / 2;
    }
    //判定是否为凸包
    bool is_convex() {
        for(int i = 0; i < n; i++) {
            if(position_points(poi[i], poi[(i + 1) % n], poi[(i + 2) % n]) == -2)
                return 0;
        }
        return 1;
    }
    //从当前点集构造凸包,改变点集的顺序为逆时针,若不想要在凸包的边上有点改为<=0
    void build_convex_hull() {
        int now = 0;
        sort(poi.begin(), poi.end());
        vector<Point> res(n << 1);
        for(int i = 0; i < n; res[now++] = poi[i++]) {
            while(now > 1 && fsign(cross(res[now - 1] - res[now - 2], poi[i] - res[now - 2])) <= 0)
                now--;
        }
        int t = now;
        for(int i = n - 2; i >= 0; res[now++] = poi[i--]) {
            while(now > t && fsign(cross(res[now - 1] - res[now - 2], poi[i] - res[now - 2])) <= 0)
                now--;
        }
        res.resize(now - 1);
        set(res);
    }
    //求凸包直径旋转卡壳
    double get_convex_diameter() {
        if(n == 2)
            return e_dis(poi[0], poi[1]);
        int a = 0, b = 0;
        for(int i = 1; i < n; i++) {
            if(poi[i] < poi[a])
                a = i;
            if(poi[i] > poi[b])
                b = i;
        }
        double max_dis = e_dis(poi[a], poi[b]);
        int now_a = a, now_b = b;
        while(now_a != b || now_b != a) {
            double now_dis = e_dis(poi[now_a], poi[now_b]);
            max_dis = max(max_dis, now_dis);
            int next_a = (now_a + 1 == n) ? 0 : now_a + 1;
            int next_b = (now_b + 1 == n) ? 0 : now_b + 1;
            if(fsign(cross(poi[next_a] - poi[now_a], poi[next_b] - poi[now_b])) < 0)
                now_a = next_a;
            else
                now_b = next_b;
        }
        return max_dis;
    }
    //判定点与多边形关系 2:点在多边形里,1:点在多边形边上,0:点在多边形外
    friend int position_polygon_point(Polygon& poly, Point p) {
        bool x = false;
        poly.poi.push_back(poly.poi[0]);
        for(int i = 0; i < poly.n; i++) {
            Point a = poly.poi[i] - p, b = poly.poi[i + 1] - p;
            if(fsign(abs(cross(a, b))) == 0 && fsign(dot(a, b)) < 1)
                return 1;
            if(fsign(a.y - b.y) > 0)
                swap(a, b);
            if(fsign(a.y) < 1 && fsign(b.y) > 0 && fsign(cross(a, b)) > 0)
                x = !x;
        }
        poly.poi.pop_back();
        return (x ? 2 : 0);
    }
    //求凸包沿直线方向向量切割后左侧的凸包
    friend Polygon get_convex_cut(Polygon& poly, Line& l) {
        vector<Point> res;
        for(int i = 0; i < poly.n; i++) {
            Point A = poly.poi[i];
            Point B = poly.poi[(i + 1) == poly.n ? 0 : (i + 1)];
            if(position_points(l.s, l.t, A) != -2)
                res.push_back(A);
            if(position_points(l.s, l.t, A) * position_points(l.s, l.t, B) < 0)
                res.push_back(get_point_corss(Line(A, B), l));
        }
        return Polygon(res);
    }
};
```

 



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
		res ^= x[i];
		res *= FNV_prime;
	}
	return res;
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

`dp[j]=max(dp[j],dp[j-w[i]]+v[i])` , 其中` dp[i] `表示背包容量为 $i$ 时能取到的最大价值

```c++
for(int i = 0; i < n; i++) {
    for(int j = W; j >= w[i]; j--) {
        dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
    }
}
```

### 多重背包

单调队列进行的优化

```c++
int que[MX];
void solve() {
    int now = 0;
    for(int i = 0; i < n; i++) {
        for(int k = 0; k < w[i]; k++) {
            int head = 0, tail = -1;
            for(int j = k; j <= W; j += w[i]) {
                while(head <= tail && j - que[head] > num[i] * w[i])
                    head++;
                while(head <= tail && dp[now ^ 1][j] >= dp[now ^ 1][que[tail]] + (j - que[tail]) / w[i] * v[i])
                    tail--;
                que[++tail] = j;
                dp[now][j] = dp[now ^ 1][que[head]] + (j - que[head]) / w[i] * v[i];
            }
        }
        now ^= 1;
    }
    cout << dp[now ^ 1][W];
}
```



### 完全背包 

`dp[j]=max(dp[j],dp[j-w[i]*k]+v[i]*k)` , 其中 `dp[j]` 表示背包容量为 $j$ 时能取到的最大价值, $k$ 为一组物品 $i$ 的数量（通常使用倍增优化）

```c++
for(int i = 0; i < n; i++) {
    int have = num[i]; //物品i的总数量
    for(int k = 1; have; k <<= 1) {
        k = min(have, k);
        for(int j = W; j >= w[i] * k; j--) {
            dp[j] = max(dp[j], dp[j - w[i] * k] + v[i] * k);
        }
        have -= k;
    }
}
```

### 超重 $01$ 背包

$w_i$ 很大但 $\sum v_i$ 很小时可以用价值作为进行转移的变量，下面给出01背包对应做法:

`dp[j]=min(dp[j], dp[j-v[i]]+w[i])` , 其中 `dp[i]` 表示当取得价值为 $i$ 时背包的最小容量

```c++
dp[0] = 0;
for(int i = 1; i <= MX; i++)
    dp[i] = ll_INF;
for(int i = 0; i < n; i++) {
    for(int j = MX; j >= v[i]; j--) {//mx为总价值
        dp[j] = min(dp[j], dp[j - v[i]] + w[i]);
    }
}
for(int i = MX; i >= 0; i--) {
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
        ll have = min(num[i], 50);
        num[i] -= have;
        for(ll k = 1; have; k <<= 1) {
            k = _min(have, k);
            for(int j = lim - 1; j >= v[i] * k; j--) {
                dp[j] = min(dp[j], dp[j - v[i] * k] + w[i] * k);
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
            ll div = min(num[id[i]], rest / w[id[i]]);
            rest -= div * w[id[i]];
            now += div * v[id[i]];
        }
        ans = max(ans, now);
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
            ans = max(ans, sumv + pre[id - 1].second);
        }
    }
    cout << ans << endl;
}
```

### 常见优化

当费用相同时，只需保留价值最高的；当价值一定时，只需保留费用最低的；更一般的，对于两件物品 $i,j$ ，若$v_i \geq v_j$ 且  $w_i \leq w_j$ ，只需保留物品 $i$ 。

## 最长子序列问题

### 最长上升子序列 LIS

​	最长不下降的话`sta[cnt] <= a[i]`改成`sta[cnt] < a[i]`

```c++
int sta[MX], cnt;
int LIS() {
    cnt = 1;
    for(int i = 0; i <= n; i++)
        sta[i] = int_INF;
    for(int i = 0; i < n; i++) {
        if(sta[cnt] <= a[i])
            sta[++cnt] = a[i];
        else {
            int j = lower_bound(sta + 1, sta + 1 + cnt, a[i]) - sta;
            sta[j] = a[i];
        }
    }
    return cnt;
}
```

### 最长下降子序列 LDS

​	最长不上升的话`sta[cnt] >= a[i]`改成`sta[cnt] > a[i]`

```c++
int sta[MX], cnt;
int LDS() {
    cnt = 1;
    for(int i = 0; i <= n; i++)
        sta[i] = 0;
    for(int i = 0; i < n; i++) {
        if(sta[cnt] >= a[i])
            sta[++cnt] = a[i];
        else {
            int j = upper_bound(sta + 1, sta + 1 + cnt, a[i], greater<int>()) - sta;
            sta[j] = a[i];
        }
    }
    return cnt;
}
```

### 数量关系 

Dilworth定理：偏序集能划分成的最少的全序集个数等于最大反链的元素个数

​	将一个序列划分成若干个单调不上升子序列的最少个数 = 该序列最长上升子序列的大小

​	将一个序列划分成若干个单调不下降子序列的最少个数 = 该序列最长下降子序列的大小

### 最长公共子序列 LCS

将 b 中的每个元素以 a 中同一元素的出现位置离散化重新编号，对处理后的 b 求 LIS 即为 LCS

### 最长公共上升子序列 LCIS

```c++
int LCIS() {
    for(int i = 1; i <= n; i++) {
        cnt = 0;
        for(int j = 1; j <= n; j++) {
            if(a[i] == b[j])
                sta[j] = cnt + 1;
            if(a[i] > b[j])
                cnt = max(cnt, sta[j]);
        }
    }
    cnt = 0;
    for(int i = 1; i <= n; i++) {
        cnt = max(sta[i], cnt);
    }
    return cnt;
}
```



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
				if (len[v] > n) 
					return 1;
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

#### SPFA + DFS

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

#### SPFA + 栈

```c++
stack<int> q;
int inq[MX], len[MX];
ll dis[MX] = {0};
int spfa_sta(int u) {
    while(!q.empty())
        q.pop();
    q.push(u);
    len[u] = dis[u] = 0;
    while(!q.empty()) {
        u = q.top();
        q.pop();
        inq[u] = 0;
        for(int i = head[u]; i; i = e[i]._next) {
            int v = e[i].v;
            if(dis[v] < dis[u] + e[i].val) {
                dis[v] = dis[u] + e[i].val;
                //判定负环（不保证出现时用 e.g.差分约束）
                len[v] = len[u] + 1;
                if(len[v] > n)
                    return 1;
                if(!inq[v]) {
                    inq[v] = 1;
                    q.push(v);
                }
            }
        }
    }
    return 0;
}
```



#### 差分约束

`addedge(u,v,val)` 在使用最短路(找最大上界)时表示 $v-u \leq val$  ，使用最长路(找最小下界)时表示 $v-u \geq val$ 。

以最短路（找最大上界）常用逻辑关系转换：( $u,v \geq 1$ ; $0$ 号节点为超级原点,值为0)

| **文字表达** |     **加边方式**     |  **文字表达**   |       **加边方式**       |
| :----------: | :------------------: | :-------------: | :----------------------: |
|   $ u>v $    |      $(u,v,-1)$      |   $ u>v +val$   |      $(u,v,-val-1)$      |
| $ u\geq v $  |      $(u,v,0)$       | $ u\geq v +val$ |       $(u,v,-val)$       |
|  $ u = v $   | $(u,v,0)$，$(v,u,0)$ |     $u=val$     | $(0,u,val)$，$(u,0,val)$ |
|   $ u<v $    |      $(v,u,-1)$      |   $ u<v +val$   |      $(v,u,val-1)$       |
| $ u\leq v $  |      $(v,u,0)$       | $ u\leq v +val$ |       $(v,u,val)$        |

以最长路（找最小下界）常用逻辑关系转换：( $u,v \geq 1$ ; $0$ 号节点为超级原点,值为0)

| **文字表达** |     **加边方式**     |  **文字表达**   |       **加边方式**       |
| :----------: | :------------------: | :-------------: | :----------------------: |
|   $ u>v $    |      $(v,u,1)$       |   $ u>v +val$   |      $(v,u,val-1)$       |
| $ u\geq v $  |      $(v,u,0)$       | $ u\geq v +val$ |       $(v,u,val)$        |
|  $ u = v $   | $(u,v,0)$，$(v,u,0)$ |     $u=val$     | $(0,u,val)$，$(u,0,val)$ |
|   $ u<v $    |      $(u,v,1)$       |   $ u<v +val$   |      $(u,v,-val+1)$      |
| $ u\leq v $  |      $(u,v,0)$       | $ u\leq v +val$ |       $(u,v,-val)$       |

剩下的就是SPFA+SLF判定负环是否存在解并计算上下界。这里判定负环的依据是**松弛次数**，需要注意的是，若可能存在重边，则需要**根据入队次数进行判负环**。

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

## 连通分量 

### 有向图的强连通分量

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
			low[u] = min(low[u], low[v]);
		}
		else if (ins[v])
			low[u] = min(low[u], dfn[v]);
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

#### 性质

- 将一个DAG变为一个强连通分量所需的最小边数为`max(入度为0的点数,出度为0的点数)`

### 无向图的双连通分量

#### 割点与桥

在判定桥时**边计数器请从1开始**

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
			low[u] = min(low[u], low[v]);
             //割点的判定
			if (low[v] >= dfn[u]) {
				now_size++;
				if (u != root || now_size > 1)
					is_cut[u] = 1;
			}
             //桥的判定
			if (low[v] > dfn[u])
				bridge[i] = bridge[i ^ 1] = 1;
		}
		else if (v != fa)
			low[u] = min(low[u], dfn[v]);
	}
}
```



### 2-SAT

`addedge(u,v)` 表示选取 $u$ 后必选取 $v$ ，常用逻辑关系转换：

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
int dfn[MX], real[MX], trcnt = 0;//以DFS序作为线段树节点,real[i]表示线段树节点i的实际编号
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

### 最大流

#### Dinic

```c++
class Network_FLow {
public:
    int N, S, T;
    void addFlowEdge(int u, int v, ll cap) {
        addedge(u, v, cap);
        addedge(v, u, 0);
    }
    void Init() {
        for(int i = 0; i <= N; i++)
            head[i] = nowe[i] = 0;
        cnt = 1;
    }
    ll Dinic() {
        ll nowflow = 0, maxflow = 0;
        while(BFS())
            while(nowflow = DFS(S, ll_INF))
                maxflow += nowflow;
        return maxflow;
    }

private:
    struct Node {
        ll cap;
        int v;
        int _next;
    } e[MX << 1];
    int head[MX], cnt = 0;
    void addedge(int u, int v, ll cap) {
        e[++cnt].v = v;
        e[cnt].cap = cap;
        e[cnt]._next = head[u];
        head[u] = cnt;
    }
    int dep[MX], nowe[MX];
    bool BFS() {
        for(int i = 0; i <= N; i++)
            dep[i] = 0;
        dep[S] = 1;
        queue<int> q;
        q.push(S);
        while(!q.empty()) {
            int u = q.front();
            q.pop();
            for(int i = head[u]; i; i = e[i]._next) {
                int v = e[i].v;
                if(e[i].cap > 0 && dep[v] == 0) {
                    dep[v] = dep[u] + 1;
                    q.push(v);
                }
            }
        }
        for(int i = 0; i <= N; i++)
            nowe[i] = head[i];
        return dep[T];
    }
    ll DFS(int u, ll flow) {
        if(u == T)
            return flow;
        ll last = flow;
        for(int i = nowe[u]; i; i = e[i]._next) {
            nowe[u] = i;
            int v = e[i].v;
            if(dep[v] == dep[u] + 1 && e[i].cap) {
                ll nflow = DFS(v, _min(last, e[i].cap));
                if(nflow > 0) {
                    e[i].cap -= nflow, e[i ^ 1].cap += nflow;
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
} flow;
```

#### LSAP

```c++
class Network_FLow {
public:
    int N, S, T;
    void addFlowEdge(int u, int v, ll cap) {
        addedge(u, v, cap);
        addedge(v, u, 0);
    }
    void Init() {
        for(int i = 0; i <= N; i++)
            head[i] = nowe[i] = 0;
        cnt = 1;
    }
    ll LSAP() {
        ll maxflow = 0;
        BFS();
        while(dep[S] <= N) {
            for(int i = 0; i <= N; i++)
                nowe[i] = head[i];
            maxflow += DFS(S, ll_INF);
        }
        return maxflow;
    }

private:
    struct Node {
        ll cap;
        int v;
        int _next;
    } e[MX << 1];
    int head[MX], cnt = 0;
    void addedge(int u, int v, ll cap) {
        e[++cnt].v = v;
        e[cnt].cap = cap;
        e[cnt]._next = head[u];
        head[u] = cnt;
    }
    int dep[MX], nowe[MX];
    void BFS() {
        for(int i = 0; i <= N; i++)
            gap[i] = dep[i] = 0;
        dep[T] = 1;
        gap[1]++;
        queue<int> q;
        q.push(T);
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
        if(u == T)
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
            dep[S] = n + 10;
        gap[++dep[u]]++;
        return flow - last;
    }
} flow;
```

### 最小费用最大流

#### Dinic

```c++
class Min_Cost_Flow {
public:
    int N, S, T;
    void addFlowEdge(int u, int v, ll cap, ll cost) {
        addedge(u, v, cap, cost);
        addedge(v, u, 0, -cost);
    }
    void Init() {
        for(int i = 0; i <= N; i++)
            head[i] = nowe[i] = 0;
        mincost = 0;
        cnt = 1;
    }
    ll Min_Cost_Dinic() {
        ll nowflow = 0, maxflow = 0;
        mincost = 0;
        while(SPFA())
            while(nowflow = DFS(S, ll_INF))
                maxflow += nowflow;
        return mincost;
    }

private:
    struct Flow_Edge {
        ll cap, cost;
        int v;
        int _next;
    } e[MX << 1];
    int head[MX], nowe[MX], cnt;
    int vis[MX];
    ll dis[MX], mincost;
    void addedge(int u, int v, ll cap, ll cost) {
        e[++cnt].v = v;
        e[cnt].cap = cap;
        e[cnt].cost = cost;
        e[cnt]._next = head[u];
        head[u] = cnt;
    }
    bool SPFA() {
        for(int i = 0; i <= N; i++)
            vis[i] = 0, dis[i] = ll_INF;
        vis[S] = 1;
        dis[S] = 0;
        queue<int> q;
        q.push(S);
        while(!q.empty()) {
            int u = q.front();
            q.pop();
            vis[u] = 0;
            for(int i = head[u]; i; i = e[i]._next) {
                int v = e[i].v;
                if(e[i].cap > 0 && dis[v] > dis[u] + e[i].cost) {
                    dis[v] = dis[u] + e[i].cost;
                    if(vis[v] == 0) {
                        vis[v] = 1;
                        q.push(v);
                    }
                }
            }
        }
        for(int i = 0; i <= N; i++)
            nowe[i] = head[i];
        return dis[T] != ll_INF;
    }
    ll DFS(int u, ll flow) {
        vis[u] = 1;
        if(u == T)
            return flow;
        ll last = flow;
        for(int i = nowe[u]; i; i = e[i]._next) {
            nowe[u] = i;
            int v = e[i].v;
            if((vis[v] == 0 || v == T) && dis[v] == dis[u] + e[i].cost && e[i].cap) {
                ll nflow = DFS(v, min(last, e[i].cap));
                if(nflow > 0) {
                    mincost += e[i].cost * nflow;
                    e[i].cap -= nflow, e[i ^ 1].cap += nflow;
                    last -= nflow;
                    if(last == 0)
                        return flow;
                }
            }
        }
        if(last == flow)
            dis[u] = ll_INF;
        return flow - last;
    }
} flow;
```

## 匹配

### 基本概念

- **匹配**：是一个边的集合，其中任意两条边都没有公共顶点

- **匹配数**：一个匹配中边的数目

- **最小点覆盖数**：选取最少的点，使任意一条边至少有一个端点被选择

- **最大独立数**：选取最多的点，使任意所选两点均为任意一条边的两个端点

- **最小路径覆盖数**：对于一个 DAG，选取最少条路径(可以为单个点)，使得每个顶点属于且仅属于一条路径。

#### 数量关系

- 最大匹配数 = 最小点覆盖数 = 最大独立数
- 最小路径覆盖数 = 顶点数 - 最大匹配数

### 判断是否是二分图

BFS染色

```c++
queue<int> q;
int color[MX] = {0};
bool check(int u) {
    q.push(u);
    color[u] = 1;
    while(!q.empty()) {
        u = q.front();
        q.pop();
        for(int i = head[u]; i; i = e[i]._next) {
            int v = e[i].v;
            if(color[v] == -1) {
                q.push(v);
                color[v] = !color[u];
            }
            else if(color[v] == color[u])
                return 0;
        }
    }
    return 1;
}
```



### 二分图最大匹配

匹配边数最多，每个点最多可匹配一次

#### 最大流做法

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

#### 匈牙利做法

```c++
int match[MX], ans[MX];
bool vis[MX];
bool DFS(int u) {
    for(int i = head[u]; i; i = e[i]._next) {
        int v = e[i].v;
        if(vis[v] == 0) {
            vis[v] = 1;
            if(match[v] == 0 || DFS(match[v])) {
                match[v] = u;
                ans[u] = v;
                return 1;
            }
        }
    }
    return 0;
}
int max_match() {
    int ans = 0;
    for(int i = 1; i <= n; i++) {
        memset(vis, 0, sizeof(bool) * (n + 5));
        ans += DFS(i);
    }
    return ans;
}
```

### 二分图最大权匹配

在边权最大的匹配中求匹配数最大的匹配方式

#### 费用流做法

最大费用最大流：建立超级源点和超级汇点，源点到x连流量为1，费用为0；x到y流量为1，费用为边权，最后流入超级汇点的即为最大匹配数，最大费用为最大权。枚举所有正边看剩余流量即可确定是否为选用的匹配边。

#### KM做法



### 一般图最大匹配

### 一般图最大权匹配
