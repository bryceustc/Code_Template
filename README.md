# 常用代码模板
整理常用的一些代码模板

#### 快速排序算法模板

```c++
void quick_sort (vector<int> &nums, int low, int high) {
    if (low < high) {
        int l = low, r = high;
        int pivot = nums[l];
        while (l < r) {
            while (l < r && nums[r] >= pivot) {
                r--;
            }
            nums[l] = nums[r];
            while (l < r && nums[l] < pivot) {
                l++;
            }
            nums[r] = nums[l];
        }
        nums[l] = pivot;
        quick_sort(nums, low, l-1);
        quick_sort(nums, l+1, high);
    }
}
```

#### 归并排序算法模板
```c++
void merge(vector<int> &nums, int low, int high) {
    int temp[N];
    int mid = low + (high - low) / 2;
    int i = 0, l = low, r = mid + 1;
    while (l <= mid && r <= high) {
        if (nums[l] < nums[r]) {
            temp[i++] = nums[l++];
        } else {
            temp[i++] = nums[r++];
        }
    }

    while (l <= mid) {
        temp[i++] = nums[l++];
    }

    while (r <= high) {
        temp[i++] = nums[r++];
    }

    i = 0;
    while (low <= high) {
        nums[low++] = temp[i++];
    }
}
```

#### 整数二分查找模板

```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;  // +1 是因为 如果l = r - 1 ,mid 还会为l，就会死循环
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```
#### 应用例题

```c++
 x的平方根
int mySqrt(int x) {
    if (x==0) return 0;
    long l = 0;
    long r = x;
    while (l < r) {
        long mid = l  + r  + 1>> 1;
        if (x / mid  >= mid) {
            l = mid;
        }  else {
            r = mid - 1;
        }
    }
    return l;
}

// 数的范围
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int main() {
    int n, q;
    cin >> n >> q;
    vector<int> nums(n);
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }
    while (q--) {
        int target;
        cin >> target;
        int l = 0;
        int r = n - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if (nums[l] != target) {
            cout << -1 << " " << -1 <<endl;
            continue;
        }
        cout << l << " ";
        l = 0;
        r = n - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= target) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        cout << l << endl;
    }
    return 0;
}
```

#### 浮点数二分查找模板
```c++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```
#### 高精度加法模板
```c++
vector<int> add(vector<int> &A, vector<int> &B) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); i++) {
        if (i < A.size()) t+=A[i];
        if (i < B.size()) t+=B[i];
        C.push_back(t%10);
        t/=10;
    }
    if (t > 0) C.push_back(t);
    return C;
}
```

#### 高精度减法模板
```c++
// (A >= B)
bool cmp (vector<int> &A, vector<int> &B) {
    if (A.size() != B.size()) return A.size() > B.size();
    for (int i = 0; i < A.size(); i++) {
        if (A[i] != B[i]) {
            return A[i] > B[i];
        }
    }
    return true;
}

vector<int> sub(vector<int> &A, vector<int> &B) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t = A[i] - t;
        if (i < B.size()) t-=B[i];
        C.push_back((t+10)%10);
        if (t < 0) t = 1;
        else t = 0;
    }

    // 去掉前导 0 （003 输出3）
    while (C.size() > 1 && C.back() == 0){
        C.pop_back();
    }
    return C;
}
```

#### 高精度乘法模板
```c++
vector<int> mul(vector<int> &A, int b) {
    vector<int> C; 
    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    if (t > 0) C.push_back(t);
    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }
    return C;
}
```

#### 高精度除法模板
```c++
vector<int> div(vector<int> &A, int b, int &r) {
    vector<int> C;  // 商
    r = 0;   // 余数
    for (int i = A.size() - 1; i >= 0; i--) {
        r = r * 10 + A[i];
        C.push_back(r /b);
        r %= b;
    }
    // 注意翻转
    reverse(C.begin(), C.end());
    // 去掉前导0
    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }
    return C;
}
```
#### 一维前缀和模板
```c++
S[i] = a[1] + a[2] + ... a[i]
a[l] + ... + a[r] = S[r] - S[l - 1]
```

#### 二维前缀和模板
```c++
/*
S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]
*/
#include <bits/stdc++.h>

using namespace std;

const int N = 1010; // 注意不能选取1e5 + 10，会爆栈
int a[N][N];
int s[N][N];

int main ()
{
    ios::sync_with_stdio(false);
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            cin >> a[i][j];
        }
    }
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= m; j++){
            s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j];
        }
    }
    while (q--) {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x2][y1-1] -s[x1-1][y2] + s[x1-1][y1-1] << endl;
    }
    return 0;
}
```

#### 一维差分模板
```c++
首先构造B[i] = A[i] - A[i-1]，这样A[i]是B的前缀和

给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c
```

#### 二维差分模板
```c++
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c
```

#### 位运算模板
```c++
求n的第k位数字: n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n
```
#### 双指针算法模板
```c++
// 滑动窗口
for (int i = 0, j = 0; i < n; i ++ )
{
    while (j < i && check(i, j)) j ++ ;

    // 具体问题的逻辑
}
常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作
```

#### 离散化模板
```c++
// 离散化的本质，是映射，将间隔很大的点，映射到相邻的数组元素中。减少对空间的需求，也减少计算量
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, ...n
}
```
#### 区间合并模板
```c++
// 将所有存在交集的区间合并
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}
```
