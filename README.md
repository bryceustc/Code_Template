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
