给定一个数组,求这个数组的第k小个数
## 直接排序
```c++
#include <iostream>
#include <vector>
#include <algorithm>

int kthSmallest(std::vector<int>& arr, int k) {
    std::sort(arr.begin(), arr.end());
    return arr[k-1];
}

int main() {
    std::vector<int> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
    int k = 4;
    std::cout << "The " << k << "th smallest element is " << kthSmallest(arr, k) << std::endl;
    return 0;
}

```
## 堆
```c++
#include <iostream>
#include <vector>
#include <queue>

int kthSmallest(std::vector<int>& arr, int k) {
    std::priority_queue<int> maxHeap;
    
    for (int i = 0; i < k; ++i) {
        maxHeap.push(arr[i]);
    }

    for (int i = k; i < arr.size(); ++i) {
        if (arr[i] < maxHeap.top()) {
            maxHeap.pop();
            maxHeap.push(arr[i]);
        }
    }

    return maxHeap.top();
}

int main() {
    std::vector<int> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
    int k = 4;
    std::cout << "The " << k << "th smallest element is " << kthSmallest(arr, k) << std::endl;
    return 0;
}
```
## 快速排序修改
```c++
#include <iostream>
#include <vector>
#include <cstdlib>

int partition(std::vector<int>& arr, int left, int right) {
    int pivot = arr[right];
    int i = left;
    for (int j = left; j < right; ++j) {
        if (arr[j] < pivot) {
            std::swap(arr[i], arr[j]);
            i++;
        }
    }
    std::swap(arr[i], arr[right]);
    return i;
}

int quickselect(std::vector<int>& arr, int left, int right, int k) {
    if (left == right) {
        return arr[left];
    }

    int pivotIndex = partition(arr, left, right);

    if (k == pivotIndex) {
        return arr[k];
    } else if (k < pivotIndex) {
        return quickselect(arr, left, pivotIndex - 1, k);
    } else {
        return quickselect(arr, pivotIndex + 1, right, k);
    }
}

int kthSmallest(std::vector<int>& arr, int k) {
    return quickselect(arr, 0, arr.size() - 1, k - 1);
}

int main() {
    std::vector<int> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
    int k = 4;
    std::cout << "The " << k << "th smallest element is " << kthSmallest(arr, k) << std::endl;
    return 0;
}
```