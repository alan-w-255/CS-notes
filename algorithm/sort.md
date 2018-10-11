# 排序算法

## 冒泡排序

> 时间复杂度 O(n^2), 空间复杂度 O(1)

<details> <summary> c 语言实现 </summary>
<p>

```c
void bubbleSort(int len, int* a) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < len -1; j++) {
      if (a[j] < a[j+1]) {
        int b = 0;
        b = a[j];
        a[j] = a[j+1];
        a[j+1] = b;
      }
    }
  }
}
```

</p>
</details>

## 堆排序

堆通常用一维数组表示. 在数据起始位置为 0 的情形中:

- 父节点 i 的左节点在位置 (2*i + 1)
- 父节点 i 的右节点在位置 (2*i + 2)
- 子节点 i 在父节点在位置 floor((i - 1) / 2)

堆定义以下几种操作:

- 最大堆调整 (Max Heapify): 将堆的末端子节点作调整, 使得子节点永远小于父节点.
- 创建最大堆 (Build Max Heap): 将堆中所有数据重新排序.
- 堆排序 (HeapSort): 移除位在第一个数据的根节点, 并作最大堆调整的递归运算.

### :lollipop: 最大堆调整 (Max Heapify)

```c++
void max_heapify(int arr[], int start, int end) {
  // 假设左右子树已经是最大堆, parent 有可能小于左右子树
  int parent = start;
  int mChild = parent * 2 + 1;
  while (mChild <= end) {
    if (mChild + 1 <= end && arr[mChild] < arr[mChild + 1]) mChild ++; // 如果有右孩子, 而且右孩子大于左孩子, 则右孩子应为新的 parent
    if (arr[parent] > arr[mChild]) return; parent 大于
    else {
      // parent 和左右孩子中最大的交换, 最大孩子成为新的 parent 进行下次的迭代.
      swap(&arr[parent], &arr[mChild]);
      parent = mChild;
      mChild = parent * 2 + 1;
    }
  }
}
```

<details><summary> c++ 实现 </summary>
<p>

```c++
#include <iostream>
#include <algorithm>
using namespace std;

void max_heapify(int arr[], int start, int end) {
  // 假设左右子树已经是最大堆, parent 有可能小于左右子树
  int parent = start;
  int mChild = parent * 2 + 1;
  while (mChild <= end) {
    if (mChild + 1 <= end && arr[mChild] < arr[mChild + 1]) mChild ++; // 如果有右孩子, 而且右孩子大于左孩子, 则右孩子应为新的 parent
    if (arr[parent] > arr[mChild]) return; parent 大于
    else {
      // parent 和左右孩子中最大的交换, 最大孩子成为新的 parent 进行下次的迭代.
      swap(&arr[parent], &arr[mChild]);
      parent = mChild;
      mChild = parent * 2 + 1;
    }
  }
}

void heap_sort(int arr[], int len) {
  for(jnt j = len / 2 -1; j >= 0; j--) max_heapjfy(arr, j, len - 1);
  for (int i = len - 1; i > 0; i--) {
    swap(arr[0], arr[i]);
    max_heapify(arr, 0, i - 1);
  }
}

int main() {
  int arr[] = {3, 5, 3, 0, 8, 6, 1, 5, 8, 6, 2, 4, 9, 4, 7, 0, 1, 8, 9, 7, 3, 1, 2, 5, 9, 7, 4, 0, 2, 6}
  int len = (int) sizeof(arr) / sizeof(*arr);
  heap_sort(arr, len);
  for (int i = 0; i < len; i++) cout << arr[i] << ' ';
  cout endl;
  return 0;
}
```

</p></details>
