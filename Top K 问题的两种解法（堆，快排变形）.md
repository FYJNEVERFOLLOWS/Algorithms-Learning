# 基于堆的Top K
使用一个大小为 k 的最大堆（大顶堆），将数组中的元素依次入堆，当堆的大小超过 k 时，便将多出的元素从堆顶弹出。由于每次从堆顶弹出的数都是堆中最大的，最小的 k 个元素一定会留在堆里。入堆和出堆操作的时间复杂度均为 $O(logk)$，每个元素都需要进行一次入堆操作，故算法的时间复杂度为 $O(nlogk)$。
​
![](https://pic3.zhimg.com/v2-7de63dfb47f845689e1ae9f48193b2ba_b.webp)

![](https://raw.githubusercontent.com/FYJNEVERFOLLOWS/Picture-Bed/main/202304/20230423211217.png)

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    int n, k;
    cin >> n >> k;
    int arr[n];
    for(int i = 0; i < n; i++) 
        cin >> arr[i];
    priority_queue<int, vector<int>, less<int> > heap; // 使用大顶堆，C++默认大顶堆，等同于priority_queue<int> heap; 
    for(int i = 0; i < n; i++) {
        if(heap.size() < k || arr[i] < heap.top()) { // 当前数字小于堆顶元素才会入堆
            heap.push(arr[i]);
        }
        if(heap.size() > k) { // 删除堆顶最大元素
            heap.pop();
        }
    }
    while (!heap.empty()) {
        cout << heap.top() << ' ';
        heap.pop();
    }
    return 0;
}
```

# 基于快排的Top K
```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int Partition(int A[], int left, int right) {
    int temp = A[left]; //将A[left]存放在临时变量temp
    while(left < right) { //left与right相遇前
        while(left < right && A[right] > temp) right--; //right不断左移
        A[left] = A[right]; //将A[right]挪到A[left]
        while(left < right && A[left] <= temp) left++; //left不断右移
        A[right] = A[left]; //将A[left]挪到A[right]
    }
    A[left] = temp; //把temp放到left与right相遇的地方
    return left; //返回相遇的下标
}
void quickSelect(int A[], int left, int right, int k) { // 找前k小的数
    int m = Partition(A, left, right); // 此时数组的前m + 1个数就是最小的m + 1个数(m是索引)
    if(k == m + 1) {
        return;
    } else if(k < m + 1) { // 最小的k个数一定在前m个数中，递归划分区间[0, m - 1]
        quickSelect(A, left, m - 1, k); 
    } else {
        quickSelect(A, m + 1, right, k);
    }
}
int main()
{
    int n, k;
    cin >> n >> k;
    int arr[n];
    for(int i = 0; i < n; i++) 
        cin >> arr[i];
    quickSelect(arr, 0, n - 1, k);
    for(int i = 0; i < k; i++) 
        printf("%d ", arr[i]);

    return 0;
}
```
时间复杂度的分析方法和快速排序类似。由于快速选择只需要递归一边的数组，时间复杂度小于快速排序，期望时间复杂度为 $O(n)$，最坏情况下的时间复杂度为$O(n^2)$。

Input Sample:
```
8 3
8 12 7 3 20 9 5 18
```
